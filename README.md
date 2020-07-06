# Slurm

## Basic commands

### Slurm node status (down -> down, idle -> ok, mix -> in use)

	sinfo 
	sinfo -N -l

### Current jobs ins Slurm

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



## Basic use of Slurm

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
	
### 
