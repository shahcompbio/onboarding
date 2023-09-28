# Containerization


Docker is not available on the cluster. Singularity is provided as an alternative. you can get singularity by running:
```
module load singularity/3.7.1
```

To run a docker image:
```
singularity run docker://<docker image url> <your command here>
```


to mount paths in singularity container:
```
singularity run --bind /work docker://<docker image url> <your command here>
```
will bind the entire work dir (hot storage) and make it accessible in the container at the same path.


You can also pull the docker image into a singularity image by:
```
singularity build dockerimage.sif docker://<docker image url>
```

then you can run any commands using this image file:
```
singularity run --bind /work dockerimage.sif <your command here>
```