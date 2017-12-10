# HaSAPPy:Ubuntu 14.04

This container runs HaSAPPy interactively in an Ubuntu 14.04 bash shell and produces output in the /data folder, which should be mapped to an external volume with the Docker -v <DATA PATH>:/data command line option.

## Usage:
```

xhost +local:root
sudo docker run -it -v <DATA PATH>:/data -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY hasappy100/hasappy:ubuntu14.04
xhost -
```


**<DATA PATH>** is used to share data between host computer and Docker HaSAPPy container file systems. The folder is mapped as volume into the container **/data folder** for storing genomes, experimental input datasets, and output of the analysis. Due to a large amount of data that will be downloaded, consider setting aside at least 200 GB of disk space. We advise not to mount the docker image storage on the disk containing the root file system but symlink /var/lib/docker to a separate large hard disk to prevent the root file system from running out of disk space.
For convenience a script is copied to <DATA PATH> for cleaning up docker images and containers. Use **cleanup-docker.sh** when running out of critical disk space.

Within the container an **init.sh script** is available to download the data and initialize the reference genome indexes. The script allows to select which downloads are required to keep the bandwidth manageable.

**Usage:**
init.sh [mouse] [human] [Monfort_2015][Jae 2013][Staring_2017]

For setting up HaSAPPy for the mouse or human genome use:
- mouse
- human

For downloading datasets associated with a published study use:
- Monfort_2015
- Jae_2013
- Staring_2017

All functionality of HaSAPPy can  be used interactively from the bash shell prompt. Documentation, source code, and and a tutorial are available from:

https://github.com/gdiminin/HaSAPPy

HaSAPPy supports the use of **Nvidia GPUs** and for some functionality depends on it. A docker image for running HaSAPPy for accessing GPUs from within the container using nvidia-docker is available:

**zanton123/hasappy:GPU**

This is the preferred solution for accessing the full functionality of HaSAPPy. A docker plugin is available at https://github.com/NVIDIA/nvidia-docker that faciliates a consistent way for GPU interaction with the container. At the time this docker plugin appears only implemented for Linux based docker hosts.

## Usage:
```

xhost +local:root
sudo docker run --runtime=nvidia -it -v <DATA PATH>:/data -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY hasappy100/hasappy:GPU
xhost -
```
