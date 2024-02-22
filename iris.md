## IRIS Cluster


The documentation for the IRIS cluster is hosted [here](https://github.mskcc.org/pages/HPC/userdocs/)

#### Shahlab Storages:

**Primary Storage**

1. We have our primary storage at `/data1/shahs3` 
2. User directories must be under `/data1/shahs3/users`. Please create your user dir as follows
    ```
    mkdir /data1/shahs3/users/<your username>
    ```
   If you're unable to run the above command due to permission errors, please contact Eli Havasov or Diljot Grewal
3. Please do not store any code environments or software in `/data1/shahs3`.


**Software Storage**
1. We have our software storage at `/usersoftware/shahs3` 
2. User directories must be under `/usersoftware/shahs3/users`. Please create your user dir as follows
    ```
    mkdir /usersoftware/shahs3/users/<your username>
    ```
   If you're unable to run the above command due to permission errors, please contact Eli Havasov or Diljot Grewal

**Scratch**
1. Scratch storage is located at `/scratch/shahs3/`
2. User directories must be under `/usersoftware/shahs3/users`. Please create your user dir as follows
    ```
    mkdir /usersoftware/shahs3/users/<your username>
    ```
   If you're unable to run the above command due to permission errors, please contact Eli Havasov or Diljot Grewal
3. This is temporary fast storage. Please consider staging frequently accessed data here prior to your pipeline run
4. The Least Recently Used data will be evicted when this fills up, so consider re-staging data here before your pipeline runs. For instance this is how mondrian pipeline stages it before running
    ```
    rsync -ravP /data1/shahs3/staging_scratch/mondrian /scratch/shahs3/
    nextflow run mondrian-scwgs/mondrian_nf -r main -params-file params.yaml -resume --output_dir results/ -profile slurm,singularity
    ```


#### Passwordless login

There are 2 ways to login without a password: 

**using kerberos single sign on:**
 as explained [here](https://github.mskcc.org/pages/HPC/userdocs/getting-Started/access.html)


**using ssh pass:**

1. Install sshpass on your computer. To install it on a macbook please follow these steps:

    1. Install Xcode  via App Store
  
    2. download source and build
    ```
        curl -O -L  https://sourceforge.net/projects/sshpass/files/sshpass/1.06/sshpass-1.06.tar.gz && tar xvzf sshpass-1.06.tar.gz
        cd sshpass-1.06/
        ./configure
        sudo make install
    ```


2. Add the following to ~/.ssh/config :
```
  Host iris
      Hostname isvlogin01.mskcc.org
      ForwardAgent yes
      ForwardX11 yes
      user your_user
```

3. Add passwords to  ~/.credentials with 600/400 permissions:
```
  export YOUR_USER=your_password
```
4. Create an alias in your ~/.bashrc or ~/.bash_profile or ~/.zshrc with the following command:
```
  source ~/.credentials
   
  iris(){
    sshpass -p $YOUR_USER ssh -A your_user@iris
  }
```
5. Once everything is setup, you can simply run the command iris and ssh to iris. (edited) 
