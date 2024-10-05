## Docker Setup

### 1. Create one Amazon Linux 2023 VM on EC2

### 2. Update the installed packages and package cache on your instance
#### sudo yum update -y

### 3. Install the most recent Docker Community Edition package
#### sudo yum install docker -y

### 4. Start the Docker service.
#### sudo service docker start

### 5. Add the ec2-user to the docker group so that ec2-user can run Docker commands without using sudo.
#### sudo usermod -a -G docker ec2-user

### 6. Logout, restart the vm

### 7.Verify that the ec2-user can run Docker commands without using sudo.

#### 8. Docker Commands

#### docker -v  --> To check Docker version installed
#### docker info --> To view a global overview of your Docker environment
#### systemctl status docker --> To check Docker is running or not
#### docker ps --> ps = process status - To Display all the running containers
#### docker ps -a --> To Display both running and stopped containers
#### docker images --> Check all the images
#### docker pull hello-world --> Pull an image from Docker Hub / Docker Registry
#### docker run IMAGE_ID  or docker run REPOSITORY_NAME --> To run Docker Image
#### cat /etc/os-release --> to check the OS details
#### exit --> to come out of a container, container will get stopped
#### docker rm container_id --> to delete a stopped container
#### docker rmi image_id --> to delete an image, we should delete the container first

#### *** -------------------------------------------------------------------------- ***

#### docker pull ubuntu ==> Pull image from DockerHub
#### docker run ubuntu  ==> Creates container from Docker Image
#### docker run -it ubuntu  ==> Creates container from Docker Image, stays inside the container
#### docker run -it --name my_ubuntu ubuntu  ==> Give a name to the container
#### docker start my_ubuntu  ==> To start a container using it's name
#### docker stop my_ubuntu   ==> To stop a container using it's name
#### docker attach my_ubuntu ==> To go inside a running container

#### docker diff ubuntu_test ==> Shows the difference of the container from base image
##### C - Change
##### A - Append
##### D - Deletion
#### docker commit ubuntu_test ubuntu1 ==> Create Docker image from a Container

#### *** -------------------------------------------------------------------------- ***

### Dockerfile --> A text file containing a set of instructions. It contains instructions to build Docker image. Application dependencies specified in Dockerfile.

#### Dockerfile Keywords:

#### FROM --> To specify base image required for our application, must be on top of a Dockerfile
	Ex: 
		FROM openjdk:11
			In the container, a Linux VM will be set up and JDK 11 gets installed.

#### MAINTAINER --> Name of maintainer / Author / Owner / Description
		Ex: MAINTAINER <test@gmail.com>

#### COPY --> To copy the file from host machine to container.

#### RUN --> To execute commands
	Executes instructions while creating Docker image.
	Ex: RUN 'sudo yum install maven'
	RUN 'git clone url'

#### CMD --> Executes instructions while creating Docker Container.
	Only last cmd will picked up, from multiple CMDs.
	Ex: CMD 'java -jar ds.jar'

#### ENV --> To set environment variables

#### WORKDIR --> To set working directory for a container

#### Steps:

##### 1: Create a file having name as Dockerfile
##### 2: Add instructions to it
##### 3: Build the Dockerfile to create the Docker image

	docker build -t test .
	-t stands for tag name, name given to Docker Image
	. means in the current directory Dockerfile is present

##### 4: Run the Docker image to create the container

#### Example 1
	FROM ubuntu
	RUN echo "Hello World !!!" > /tmp/sample_file.txt
 
#### Example 2
	FROM ubuntu
	WORKDIR /tmp
	ENV institute_name IDREAM
	COPY file1.txt /tmp
 
#### Run Tomcat Server On Docker Container and do the port mappint

Information taken from: https://forums.docker.com/t/tomcat-give-error-404/95130

##### If the information which is provided on Tomcat image official documentation in DockerHub will not run Tomcat properly, then follow these steps:
##### Based on the community request, Webapps folder is moved to the webapps.dist folder, which means webapps folder is empty and there are no files to serve on the browser. 
##### That is when you saw the error message The origin server did not find a current representation for the target resource or is not willing to disclose that one exists.
##### To re-enable the webapps, we need to move the files from webapps.dist to webapps. Here are the steps to run to make sure that the tomcat server is up and running without any issues.

	docker pull tomcat:latest
	docker run -d --name mytomcat -p 8080:8080 tomcat:latest
	docker exec -it mytomcat /bin/bash
	mv webapps webapps2
	mv webapps.dist/ webapps
	exit


#### Create Image for spring boot application
	FROM openjdk
	COPY *.jar /
	ENTRYPOINT [ "java", "-jar", "demo-1.0.0.jar" ]












































