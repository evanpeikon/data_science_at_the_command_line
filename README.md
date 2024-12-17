# Data Science at the Command Line

## Getting Started
This book uses many different command-line tools. Many of these commnand-line tools are pre-installed with Unix, or are available in packages. However, this book will also use tools that are not avaiable as packages and require manual instalation. In order to aquire these tools without having to go through the installation process of each, you can install the Docker image created for this book. 

A Docker image is a bundle of one or more applications together with all their dependencies, and a Docker container is an isolated environment that runs an image. To install the Docker image for this book, you first need to download and install Docker itself from the Docker website. Once Docker is installed, you invoke the following command on your terminal or command prompt to download and run the Docker image (you can run this code from any directory):

```bash
# download docker image
$ docker pull datasciencetoolbox/dsatcl2e
# run docker image. -rm flag ensures container is deleted when you exit (no leftover state). Exit container with Ctrl+D
$ docker run --rm -it datasciencetoolbox/dsatcl2e
```

After executing the code above you're inside an isolated environment—known as a Docker container—with all the necessary command-line tools installed. If you want to get data in and out of the Docker container, you can add a volume, which means that a local directory gets mapped to a directory inside the container (Note: volumes allow you to share files between your host machine and the Docker container). 

For example, as i'm working through this book I have a folder on my desktop and inside that folder I have a sub-directory named dsatcl, which houses all of the data for this book. Now, if I want to navigate to my Comp_Bio folder and mount that directory so I can load local data into the Docker container, i'd use the following code:

```bash
$ cd Desktop/Comp_Bio
$ docker run --rm -it -v "$(pwd)":/data datasciencetoolbox/dsatcl2e
```

In the code above, ```-v "$(pwd)":/data``` mounts the current working directory (Comp_Bio) to the ```/data``` directory inside the Docker container. Now, when inside the Docker container you can access everything in the Comp_Bio directory, including all of the data from this book (ie, the code above allows the Docker container to access both Comp_Bio and all subdirectories within it). For example, I can now run the following code inside the Docker container, and recieve the corresponding outputs:

```bash
$ pwd
/home/dst
$ cd /data
$ ls
Biostar   cell_line_to_command_line_v2   dsatcl  'Machine Learning'
```

> Note: in the example above ```/home/dst/``` is the default directory in the docker container from this book, and after using the ```ls``` command in the data directory I can view all of the sub-directories that were in my Comp_Bio folder inside the Docker container. 
 
## Obtaining Data
Begin by navigating to the directory where the data from this book is stored, then navigate to the chapter 3 sub-foder, as demonstrated in the code below (use your own file path):

```bash
$ cd ~/Desktop/Comp_Bio/dsatcl/dsatcl-data/ch03
$ ls -l
total 0
-rw-r--r--@ 1 evanpeikon  staff   626K Mar  9  2023 logs.tar.gz
-rw-r--r--@ 1 evanpeikon  staff   189K Mar  9  2023 r-datasets.db
-rw-r--r--@ 1 evanpeikon  staff   149B Mar  9  2023 tmnt-basic.csv
-rw-r--r--@ 1 evanpeikon  staff   148B Mar  9  2023 tmnt-missing-newline.csv
-rw-r--r--@ 1 evanpeikon  staff   181B Mar  9  2023 tmnt-with-header.csv
-rw-r--r--@ 1 evanpeikon  staff    91K Mar  9  2023 top2000.xlsx
```

### Copying Local Files to the Docker Container
A common situation is that you already have the necessary files on your own computer. This section explains how you can get those files into the Docker container.

As mentioned previously, a Docker container is an isolated virtual environment. However, there is one exception to that: files can be transferred in and out of the Docker container

 mentioned in Chapter 2 that the Docker container is an isolated virtual environment. Luckily there is one exception to that: files can be transferred in and out the Docker container. The local directory from which you ran docker run, is mapped to a directory in the Docker container. This directory is called /data. Note that this is not the home directory, which is /home/dst.

If you have one or more files on your local computer, and you want to apply some command-line tools to them, all you have to do is copy or move the files to that mapped directory. Let’s assume that you have a file called logs.csv in your Downloads directory.


