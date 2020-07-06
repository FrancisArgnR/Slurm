# Slurm

## Table of Contents
====================

* [Basic commands](#basic-commands)
* [Examples of use of Slurm](#examples-of-use-of-slurm)

====================

## Basic commands

### Slurm node status (down -> down, idle -> ok, mix -> in use)

	sinfo 
	sinfo -N -l

### Current jobs in Slurm

	squeue

### Launch a job

	srun

### Cancel Slurm jobs

	scancel <jobid>
	scancel -u <username>
	scancel -w <nodename>

### Check the Slurm state in the node

	service slurmd status

### Raise Slurm node if it appears down after a reboot

	scontrol update nodename=NODE_NAME state=IDLE



## Examples of use of Slurm

### Launches in 3 nodes of the default partition the command (replicates the command)(in the foreground)

	srun -N3 -l /bin/hostname	
	
### Launches in 4 tasks from 1 node of the default partition the command (replicates the command)(in the foreground)

	srun -n4 -l /bin/hostname
	
### Launches on 1 node of the "ulises" partition (node103) the script R rprueba.R (in the foreground)

	srun -N1 -p "ulises" -w "node103" -e Slurm.error -o Slurm.out Rscript rprueba.R
	
### Same as above but in the background

	srun -N1 -p "ulises" -w "node103" -e Slurm.error -o Slurm.out Rscript rprueba.R &
	
### It is similar to the previous one, but it specifies the use of 2 CPUs per task

	srun -N1 -c2 -p "ulises" -w "node101" -J "test" -o "res.txt" -e "error.txt" Rscript rprueba.R &
	
### Launches on 1 node of the "ulises" partition (any but node103) the script R rprueba.R (in the foreground)

	srun -N1 -p "ulises" -x "node103" -e Slurm.error -o Slurm.out Rscript rprueba.R
	
### Launch a job from a script

#### Simple example

	prueba.sh:

		#!/bin/bash			# Specification of where it will run (if there are problems it may be another interpreter)	
		Rscript PruebaSlurm.R		# Code with the sentence: R.Version()$version.string
		
	sbatch prueba.sh

#### Complex example

	myscript.sh:
	
		#!/bin/bash			# Specification of where it is to be executed (may generate problems if the interpreter is another one)
		#
		#SBATCH --job-name=test		# Name of the job
		#SBATCH --output=res.txt	# File to save the output
		#SBATCH --error=error.txt	# File in which error messages are stored
		#SBATCH --partition=ulises	# Partition on which to launch the job (if omitted, the default partition is used)
		#
		#SBATCH --nodes=1		# No. of nodes in which it is launched (if not specified, it is determined by slurm)
		#SBATCH --nodelist=node103	# Names of specific nodes to host the job (if not specified, determined by slurm)
		#SBATCH --ntasks=10		# Number of tasks in total (to be distributed equally among the nodes)
		#SBATCH --ntasks-per-node=	# Number of tasks per node
		#SBATCH --cpus-per-task=4  	# CPUs to use per task (interesting if working in multithread (default one CPU per task))

		srun Rscript rprueba.R

	sbatch myscript.sh

