# Slurm

## Basic commands

### Slurm node status (down -> down, idle -> ok, mix -> in use)

	sinfo 
	sinfo -N -l

### Current jobs ins Slurm

	squeue

### Cancel Slurm jobs

	scancel <jobid>
	scancel -u <username>
	scancel -w <nodename>

### Check the Slurm state in the node

	service slurmd status

### Raise Slurm node if it appears down after a reboot

	scontrol update nodename=NODE_NAME state=IDLE



## Basic use of Slurm
