You can log into juno with
```
ssh juno.mskcc.org
```
To sign into the shahlab head node for the cluster:
```
ssh ceto.mskcc.org
```
Check out documentation at hpc.mskcc.org and HPC for more details. 

`juno.mskcc.org` is a general use node with restrictions on cpu and memory usage, and is only suited for very lightweight processes such as pipeline launches. `ceto.mskcc.org` is exclusive to shahlab and has no restrictions. In most cases, you can just use ceto.



## bsub
Please remember that all computationally intensive jobs should be submitted through `bsub`, which will submit your jobs into the compute cluster.<br>
You'll likely pet peeves for the HPC team if you submit large jobs in the login node (like juno or ceto), and if worse your job might eat up all the memory or CPUs of the cluster that the login node will be unaccessible for all other users.<br>
Some useful commands are: `bsub` for job submission, `bjobs` for reviewing your jobs, `bqueues` for checking queues, `bjobs -l $BSUB_JOB_ID` for detailed job info.<br>
You can see the `bsub` manual by using `man bsub`, but here are some example usages.

### 1. Wrapping your command with `bsub`
```bash
THREADS=12
MEMORYCAP=60  # max memory 60G
WALLTIME=24:00  # 24 hours
JOBNAME=test
STDOUT_FILE=/path/to/myjob.out
STDERR_FILE=/path/to/myjob.err
CMD="some command that DOES NOT have stdin or stdout brackets"
bsub -n $THREADS -M $MEMORYCAP -W $WALLTIME -J $JOBNAME -o $STDOUT_FILE -e $STDERR_FILE $CMD
```

### 2. Using a standalone script
You can create a bash script like the following and run by `bsub < script.sh`
```bash
#!/bin/bash
#BSUB -n 12
#BSUB -M 60
#BSUB -W 24:00
#BSUB -J test
#BSUB -o /path/to/myjob.out
#BSUB -e /path/to/myjob.err

module load singularity/3.6.2

singularity run -B /juno docker://some/image \
some complex commands \
that can have \
stdout and stderr in the command lines \
> like.this.out \
2> like.this.err
```
