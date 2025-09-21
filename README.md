# DevOps-Example
This is a sample Spring Boot Application, used to explain the Jenkins pipeline, in creating a full CI/CD flow using docker too.

# Jenkins 
Jenkins is an open source automation server written in Java. Jenkins helps to automate the non-human part of the software development process,
 with continuous integration and facilitating technical aspects of continuous delivery. It is a server-based system that runs in servlet containers 
 such as Apache Tomcat.
 
# Docker 

Docker is a collection of interoperating software-as-a-service and platform-as-a-service offerings that employ operating-system-level virtualization 
to cultivate development and delivery of software inside standardized software packages called containers. The software that hosts the containers 
is called Docker Engine.

# Using Jenkins with Docker
First of all to use jenkins with docker, we will have to know that as we are running Jenkins inside a docker container, and we need access to docker to
build our services images, so first of all let's build a Jenkins image with docker installed inside it. Let's check the dockerfile to 
build this Jenkins image with docker inside, you will just have to put this file into any folder, and run the docker build command.

# Step -1 Create EC2 on aws update packages and install docker with the help of this command.
Sudo apt-get update && sudo apt install docker.io -y

#  Step-2 Create Dockerfile 
Sudo -i 
Vi Dockerfile
# Dockerfile for Jenkins
from jenkins/jenkins:lts
USER root
RUN apt-get update -qq \
    && apt-get install -qqy apt-transport-https ca-certificates curl gnupg2 software-properties-common
RUN curl -fsSL https://download.docker.com/linux/debian/gpg | apt-key add -
RUN add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/debian \
   $(lsb_release -cs) \
   stable"
RUN apt-get update  -qq \
    && apt-get install docker.io -y
RUN usermod -aG docker jenkins


# Step-3 Now build the Image 

$ docker image build -t jenkins-docker .

# Step-4 Now that the docker image has already been built, we can run the Jenkins in a docker container with the command:

$ docker container run -d -p 8080:8080 -v /var/run/docker.sock:/var/run/docker.sock jenkins-docker

# Make sure port 8080 open in firewall/security group

# Step-5 Then we go insdie a container. 
$ Docker exec-it <container_id> /bin/bash

# For jenkins deafult password we use.
$ cat /var/lib/jenkins/secrets/initialAdminPassword

# Create a pipeline job and CI push to the docker hub. 
$ Click on configure and define the which you choose Pipeline script from scm or pipeline script 
$ if you choose scm give github repo link otherwise write script of pipeline in groovy code.

# Click on setting after that click on tools at end add maven version like maven-3.5.2

# Step-6 Click on build now you show one error check in console output. 
$ Missing docker pipeline plugin

# Step-7 Click on setting and then click on plugin install docker pipeline plugin.
$ Install docker plugin

# Step-8 Click again build now and check and verify your image come or not.

# Step-9 Configure Docker Build and Publish (Continuous Deployment - CD).

# Step-10 Configure the Docker registry credentials.

# Step-11 Verify the logs on the Jenkins console and verify on Docker Hub.

# Final Outcome
$ The final outcome of this pipeline is a Docker image that contains your application and all its dependencies. This image is now stored in a central registry, making it easy for any team or environment to pull and run the application consistently.

The image contains:
(.) Your application's jar file (a Java application).
(.) The necessary Java runtime libraries.
(.) The operating system (OS) environment needed to run the application.
