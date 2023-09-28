# Transfer Host / Qbio


To transfer data to/from external systems to MSK clusters, please request a new Qbio account at: 

https://thespot.mskcc.org/esc?id=sc_cat_item&sys_id=18c2aeeddbbd1110b4d51619139619f5

If this link does not work, please go to thespot.mskcc.org and search `qbio`

Qbio is accessible at `qbio.mskcc.org`. Your public key should work if the account is setup properly.


Expedat Usage: 

The Qbio account should also come with an expedat account and a password.

You can find the Expedat movedat tool at:

Juno:
`/opt/common/CentOS_6-dev/ExpeDat/ClientFiles/movedat`



Usage Scenario:
Lets assume that we need to move data from BCCRC cluster in BC, Canada to the cluster here in MSK.


 go to the transfer host in shahlab cluster and run:
```
./client/ClientFiles/movedat /path/to/file/in/shahlab {username}@qbio.mskcc.org:/path/to/file/in/qbio
./client/ClientFiles/movedat /path/to/file/in/shahlab {username}:{password}@qbio.mskcc.org:/path/to/file/in/qbio
```

Enter your expedat password and transfer should start. you can also specify your password on the commandline if you're running in batch mode.

2. Go to xbio and check if your files were transferred properly. The files will be stored in `/data2/shah`

3. go to juno and transfer from qbio to juno:
```
/opt/common/CentOS_6-dev/ExpeDat/ClientFiles/movedat {username}@qbio.mskcc.org:/path/to/file/in/qbio /path/to/file/in/juno
```
4. delete the files from qbio. This part is tricky, the files arent owned by the user, so users cant delete the files by logging into qbio and running `rm`. The trick is to use movedat to delete files
```
/opt/common/CentOS_6-dev/ExpeDat/ClientFiles/movedat {username}@qbio.mskcc.org:/path/to/file/in/qbio
```
unfortunately movedat can't delete directories. so I normally run

```
find <dir-name-here>
```
on qbio and redirect the output to a file. copy the file to juno and then iterate until all files and directories are deleted.

```
cat files-to-delete | while read x; do /opt/common/CentOS_6-dev/ExpeDat/ClientFiles/movedat {username}@qbio.mskcc.org:$x; done
```
this will throw errors when it tries to delete files that it can't. just ignore the error and run it inside a while look until all files are deleted and then kill it the process.
