# Docker Setup

## 1. Create one Amazon Linux 2023 VM on EC2

## 2. Update the installed packages and package cache on your instance
### sudo yum update -y

## 3. Install the most recent Docker Community Edition package
### sudo yum install docker -y

## 4. Start the Docker service.
### sudo service docker start

## 5. Add the ec2-user to the docker group so that ec2-user can run Docker commands without using sudo.
### sudo usermod -a -G docker ec2-user

## 6. Logout, restart the vm

## 7.Verify that the ec2-user can run Docker commands without using sudo.
### docker -v
### docker info
### docker ps
### docker images
### docker pull hello-world
### docker run IMAGE_ID



