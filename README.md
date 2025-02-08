# Docker Tutorials

# Part I: Docker Basics: What is Docker and Basic Docker Commands

Collection of programs is called as software project

-> Software project contains several components

		1) Front end components (User interface logic)

		2) Backend components (Business Logic)

		3) Database Components (Persistence Logic)


-> In order to deploy our application in a machine we need to setup all the Softwares which are required to our application

			Ex: OS, Java 1.8v, MYSQL DB, Tomcat Web Server 9.0v etc.....

-> In Realtime project should be deployed into multiple environments for testing purpose

			Ex : DEV, SIT, UAT, PILOT and PROD


-> DEV env will be used by Developers to perform integration testing

-> SIT env will be used by Testing team to test functionality of the application

-> UAT env will be used by Client to test functionality of the application

-> PILOT env means pre-production testing env

-> PROD means live environment (It is used to deliver the project)


-> To deploy application to these many enivornments we need to take of all the softwars required to run our application in all environments. It is very difficult task.

Virtualization
---------------

-> Installing Multiple Guest Operating Systems in one Host Operating System 

-> Hypervisior S/w will be used to achieve this

-> We need to install all the required softwares in HOST OS to run our application

-> It is old technique to run the applications

-> System performance will become slow in this process

-> To overcome the problems of Virtualization we are going for Containerization concept

Containerization
------------------
-> It is used to package all the softwares and application code in one container for execution

-> Container will take care of everything which is required to run our application

-> We can run the containers in Multiple Machines easily

-> Docker is a containerization software

-> Using Docker we will create container for our application 

-> Using Docker we will create image for our application

-> Docker images we can share easily to mulitple machines

-> Using Docker image we can create docker container and we can execute it

Conclusion
---------------

-> Docker is a containerization software

-> Docker will take care of application and application dependencies for execution

-> Deployments into multiple environments will become easy if we use Docker containers concept

Install Docker in Amazon Linux
------------------------------------
$ sudo yum update -y

$ sudo yum install docker -y

$ sudo service docker start

### add user to docker group by executing below command
$ sudo usermod -aG docker ec2-user

$ docker info

### Restart the session
$ exit


Docker Commands
---------------

### see docker info 
$ docker info

### To see docker images execute below command
$ docker images

### Pulling hello-world docker image 
$ docker pull hello-world

### see docker image 
$ docker images

### Running hello-world docker image 
$ docker run hello-world


# Part II: Docker Basics: Create Docker File and Images

Create account in docker hub
------------------------------

https://hub.docker.com/

Docker file
-------------

-> Docker file contains instructions to create docker image. Which contains docker domain specific keywords
to build the docker image

Docker image
--------------

-> It is a package which contains everything to run your application

Docker container
---------------

-> It is an instance of docker images, if you run docker image container will be created. That's where our
application is running

Docker registry
-----------------
We can store and share the docker images


public registery
---------------

Docker hub is a public repository, which contains all the open source softwares as a docker images.
we can think of docker hub as play store for docker images

private registry
-------------------
 
As a private registries we can use Nexus, Jfrog etc..

Docker Engine or Docker Daemon or Docker Host
---------------------------------------------

It is a software using which we can create images and containers

$ vi Dockerfile

FROM ubuntu

RUN echo "Welcome to Docker Session"

RUN echo "Welcome to Kubernetes Sessions"

CMD echo "DevOps"

CMD echo "AWS"

RUN echo "DevOps wit AWS"


-> Build docker image using dockerfile

$ docker build -t raju .

-> see docker images

$ docker images

-> Tag docker image

$ docker tag raju mraju25/raju

-> Login into docker account

 $ docker login

  username:
  
  password:

 Login successful

-> Push the tagged image into docker hub

 $ docker push mraju25/raju

=================================================================

**-> Dockerfile contains instructions to build docker images**

**-> Dockerfile will use domain specific language keywords**

**-> Docker engin will process dockerfile instructions from top to bottom**

Dockerfile keywords
-------------------
```
FROM
MAINTAINER
COPY
ADD
RUN
CMD
ENTRYPOINT
ENV
LABEL
USER
WORKDIR
EXPOSE
VOLUME
```

FROM
======
FROM : It indicates base image to run our application. On top of base image we will create our own image

Syntax : FROM <IMAGE-NAME>

Example : 
```
FROM tomcat:9.2
FROM java:jdk-1.8.0
FROM mysql
```

MAINTAINER
============
-> It represents who is author of Dockerfile

Ex :   MAINTAINER  Raju 


COPY
=====
-> It is used to copy files / folders to image while creating an image

Syntax :    COPY <source> <destination>


Example : 

### copying war file from target directory to tomcat/webapps directory

COPY target/maven-web-app.war  /usr/local/tomcat/webapp/maven-web-app.war


ADD
====

-> ADD is also used to copy files to image while creating an image

-> ADD keyword can download files from remote location (http)

-> ADD keyword will extract tar file while copying to imgae

Note: zip files we have to extract manually


Syntax : 

ADD <source> <destination>

ADD <url-to-download> <destination>


Q) What is the difference between COPY and ADD ?


RUN
====

-> It is used to execute commands on top of base image

-> Run command instructions will execute while creating an image

-> We can write multiple RUN instructions, they will execute in the order (from top to bottom)

Example :

RUN mkdir  workspace

RUN yum install git


CMD
====

-> CMD is also used to execute commands

-> CMD instructions will execute while creating container

Example : 

CMD sudo start tomcat

-> We can write multiple CMD instructions in Dockerfile but Docker will process only last CMD instruction.

Note: There is no use of writing multiple CMD instructions in Dockerfile


Sample Dockerfile
==================
```
FROM ubuntu
MAINTAINER  Ajit
RUN echo "DevOps"
RUN echo "AWS"
CMD echo "Docker"
CMD echo "Kubernetes"
RUN echo "Java"
```

### build image using docker file
$ docker build -t myimage1 .

### Run image
$ docker run myimage1 

Note: CMD instruction we can override using runtime CMD

#It will print only date (CMD will not execute)
$ docker run myimage1 date

### We can change docker file name
$ mv Dockerfile Dockerfile_1

### Creating Docker image using Dockerfile_One
$ docker build -f Dockerfile_1 -t myimage2 .


ENTRYPOINT
==============
-> ENTRYPOINT instructions will execute while creating container

Note: CMD instructions we can override where as ENTRYPOINT instructions we can't override

Example"

ENTRYPOINT [ "echo", "Welcome to DevOps classes "]


WORKDIR
==========
-> It is used to set Working Directory for an image / container

Ex: WORKDIR <DIR-PATH>

Note: The Dockerfile instructions which are available after WORKDIR those instructions will be proess from given working directory

ENV
====

-> ENV is used to set Environment Variables

Ex:  ENV <key> <value>

LABEL
======

-> LABEL will represent data in key value pair

-> It is used to add meta data for our image

Ex: LABEL branchName  release

ARG
=====

-> It is used to avoid hard coded values in Dockerfile

Ex: 

ARG branch=develop
LABEL branch $branch

Note: We can pass argument values in RUNTIME

$ docker build -t myimage1 --build-arg branch=feature

USER
====
->  We can set user for an image / container

Note: After USER instruction, remaining instructions will be processed with given USER

EXPOSE
========
-> It represents on which port number our container is running

-> It is just like a documentation to understand container running port number

VOLUME
========
-> It is used for data storage


## Part III: Docker Advance: Create Real Application Docker Images and Deploy to Kubernates cluetrs (AKS / EKS / ECS )

### 1. Create Docker Image for NodeJS app
	
**Pre-requisite:**

1) NodeJS
```
   npm install -g npm
   sudo npm install -g npm
   curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
   . ~/.nvm/nvm.sh
   nvm install --lts
   npm -v
   node -e "console.log('Running Node.js ' + process.version)"

https://docs.aws.amazon.com/sdk-for-javascript/v2/developer-guide/setting-up-node-on-ec2-instance.html
```
2) Docker
```
sudo dnf update
sudo dnf install docker
sudo systemctl start docker
sudo systemctl enable docker
sudo systemctl status docker

sudo usermod -aG docker jenkins
newgrp docker

docker pull ubuntu
docker create -it --name demo ubuntu  ### Create container:
docker start demo  ### Start container
docker attach demo  ### Get the command line of the installed container:

sudo dnf remove docker  ### Docker Uninstallation (optional)
``` 

**Create NodeJS App Image:**
``` 
$ git clone https://github.com/Anshuman2121/react_app.git
$ touch Dockerfile
$ vi Dockerfile
$ cat Dockerfile
FROM node:20.9.0-alpine
WORKDIR /app
COPY . /app
RUN npm install
RUN npm run build
```

**Build docker image using dockerfile**
```
# Build docker image
$ docker build -t ajit .
# docker images
$ docker images
# Tag docker image
$ docker tag ajit ardher/ajit
# Login into docker account
 $ docker login
# Push the tagged image into docker hub
 $ docker push ardher/ajit
``` 


