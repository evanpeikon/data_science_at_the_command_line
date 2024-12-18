# Data Science at the Command Line

## 🐍 Getting Started With Docker
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
 
### Copying Local Files to the Docker Container
A common situation is that you already have the necessary files on your own computer. This section explains how you can get those files into the Docker container. 

As mentioned previously, a Docker container is an isolated virtual environment. However, there is one exception to that: files can be transferred in and out of the Docker container. The local directory from when you executed the ```docker run``` command is mapped to a directory inside the Docker container (in the example above, ```/data```). Now, if I have any files on my local computer, I can transfer them into the mapper directory (```Comp_Bio``` in the example from the last section), and then they will be available in the Docker container under ```/data```. 

> Note: In practice I may not want to just transfer a local file into the Comp_Bio folder on my Desktop since I may have trouble finding it later (assuming it was originally elsewhere for a reason). Instead, I can copy the desired file, then put the copy in Comp_Bio, enabling me to run command-line tools on it in the Docker container.

## 🐍 Creating Command-Line Tools

There are many command-line tools that can be executed with a single line of code. These are commonly called one-liners or pipelines. The ability to perform complex tasks with a single one-liner is one of the most powerful features of working in the command-line. 

In bioinformatics you'll encounter some tasks that you perform just once, which tend to be very specific to a single dataset, and others that you repeat frequently which can be generalized to many experiments. If you find yourself repeating a certain one-liner regularly, say, for quality control of sequencing reads or formatting genomic data, it’s worth turning it into a reusable command-line tool. Both one-liners and command-line tools have their place, and learning when to use each takes experience.

In this section we'll explore how to turn useful one-liners into robust, reusable command-line tools. 

### Converting One-Liners into Shell Scripts

For bioinformatics scientists, many workflows involve repetitive tasks like processing text, parsing sequences, or analyzing large datasets. Automating these tasks can save significant time and effort. In this sub-section, we’ll take a one-liner command that processes text data and turn it into a reusable shell script—an essential skill for building command-line tools.

**Scenario**: Let’s say we want to extract the most frequently occurring words in a dataset, such as a genome annotation file or a research paper. In this case study we'll explore how to  convert a one-liner into a reusable scrip, using the text of Alice’s Adventures in Wonderland as an example. 

