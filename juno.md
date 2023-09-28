# Juno

## Available Hardware:

### Shahlab machines:

| Physical Cores | Virtual Cores | Memory | GPU         | Count | Type       | Name |
|----------------|---------------|--------|-------------|-------|------------|------|
| 36             | 72            | 500    | 0           | 24    | Compute    | jb*  |
| 36             | 72            | 500    | 4 (RTX2080) | 2     | Compute    | jc*  |
| 36             | 72            | 500    | 0           | 1     | Head Node  | ceto |

#### Storages:

| Location              | Capacity | Type                | Purpose                           |
|-----------------------|----------|---------------------|-----------------------------------|
| `/work/shah`          | 1.25PiB  | Hot                 | compute/ open to all users        |
| `/warm/shah`          | 220T     | warm                | archive/ open to all users        |
| `/ifs/rtsia01/shah/`  | 400T     | warm                | archive / restricted              |
| `/oscar/backup/shah/` | 100T     | warm (with backups) | archive / open to all users       |

### Externally owned machines:
```
3096 physical cores
6192 virtual cores
```

## Access:

### Shahlab machines
All shahlab users should have priority access to the shahlab owned machines. Please check if you have access by running the following command:
```
bugroup | grep SHAH
```
Please ensure your username is listed in the output. If it isnt please reach out to HPC in #chat-with-hpc channel in slack and request to be added to "Shahlab LSF group".

Other users/groups can run only 'short' jobs on our nodes and ONLY when they're idle. They can also only ever use up to 80% of your nodes, so there are always systems reserved exclusively for your group.

### External machines
we have low priority access to `externally owned machines` nodes. That generally translates to: your job is eligible for scheduling on these nodes if walltime is under 6 hours.


### Head nodes
There are 2 login nodes on the juno cluster for shahlab users:

```ssh juno.mskcc.org```
and
```ssh ceto.mskcc.org```

If you have an account, then the login should work for both.

We recommend ceto over juno headnode for the following reasons:
- Ceto node is for shahlab users only, so it has a smaller userbase
- ceto has faster interconnect
- juno has restrictions on cpu and memory usage, and is only suited for very lightweight processes such as pipeline launches. ceto has no restrictions.


Please remember that all computationally intensive jobs should be submitted through bsub, which will submit your jobs into the compute cluster.
You'll likely pet peeves for the HPC team if you submit large jobs in the login node (like juno or ceto), and if worse your job might eat up all the memory or CPUs of the cluster that the login node will be unaccessible for all other users.
### Storage

#### Hot Storage
- location: '/work/shah'. Please create a user dir for yourself at `/work/shah/users/<userid>`
- The hot storage should be used to store only the active datasets.
- Since hot storage costs more than warm storage, please avoid using it as long term storage.


#### User Warm Storage
- `/warm/shah` can be used as long term archival storage.
- please create a user dir for yourself if needed at `/warm/shah/<userid>`
- `/oscar/backup/shah/` is also available for warm storage if backups are required. Please create a dir for yourself at `/oscar/backup/shah/<userid>`

Please create a directory for yourself and keep your data inside that directory. Any files outside of user directories will be deleted. 
If you need more hot storage, please contact Eli Havasove or Andrew Mcpherson.



## SSH config:
To make it easier to login you can add the following to your .ssh/config file
```
Host ceto
  User <username>
  Hostname ceto.mskcc.org
  ForwardAgent yes
```
Then you can just login as follows:
```
ssh ceto
```

# Tips and Tricks

### list hosts
- Once you're on the cluster you can run `bhosts` to list all available nodes.
- The nodes starting with jb or jc are owned by shahlab. 
- All jb machines have 72 cores and 500 GB of memory. 
- The 2 jc machines have 72 cores, 500GB memory and 4 RTX2080 blower GPUs. 
- All nodes starting with ja, jd, jv, jx are external nodes. 

### list jobs
- `bjobs` will list all running jobs
- `bjobs -u <user id>` will list all running jobs by user
- `bjobs -u all` will list all running jobs for all users
- `bjobs -l <jobid>` will provide detailed logging info about a job. This is available after job has completed and can be used to debug errors.

### Interactive sessions

- This will start an interactive session that will last for 4 days:
```
bsub -W 96:00 -Is /bin/bash
```

- To add memory request:
```
bsub -W 96:00 -n 1 -R "rusage[mem=20]" -Is /bin/bash
```
- request 2 virtual cores (mem request in this case is per core)
```
bsub -W 96:00 -n 2 -R "rusage[mem=20]span[hosts=1]" -Is /bin/bash
```
- add gpu 
```
bsub -sla jcSC -q gpuqueue -gpu "num=1"  -W 96:00 -n 2 -R "rusage[mem=20]span[hosts=1]" -Is /bin/bash
```


### Submit jobs

You can see the `bsub` manual by using `man bsub`. There are a few ways to submit:


- write your command in a shell script and then run:
```
bsub -W 96:00 -n 2 -R "rusage[mem=20]span[hosts=1]"  </path/to/script/with/command>
```

- request 1 GPU, 2 cpu cores and 20 gigs:
```
bsub -W 96:00 -sla jcSC -q gpuqueue -gpu "num=1" -n 2 -R "rusage[mem=20]span[hosts=1]"  </path/to/script/with/command>
```


- Template Submission :
  - Start bsub lines with #
  - rusage specifies memory per core, not total memory needed. 
  Actually memory requirements will be number of cores X mem per core.
  - W specifies a hard walltime, the submitted job will be killed if this limit is exceeded.
  Format can be in hours:mins or just mins.  Ex: 1:30 or 120
  - We specified a soft walltime, this is just used as a scheduling estimate.
  - span can be used to configure how lsf will allocate cores for your job.
  hosts==1, allocates all cores on a single node.
  span[block=6], allocates cores on nodes in blocks of 6.
  - select can be used to specify the OS type. All new nodes should be centos 7.

    ```
    #BSUB -J "NEWJOB"
    
    #BSUB -o "/juno/work/shah/ceglian/newjob/std.out"
    
    #BSUB -e "/juno/work/shah/ceglian/newjob/std.err"
    
    #BSUB -R "rusage[mem=4]"
    
    #BSUB -n "16"
    
    #BSUB -W 01:00
    
    #BSUB -R "select[type==CentOS7]"
    
    #BSUB -R span[hosts=1]
    
    CMD
    ```
- Wrapping your command with `bsub`
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
- You can create a bash script like the following and run by `bsub < script.sh`
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