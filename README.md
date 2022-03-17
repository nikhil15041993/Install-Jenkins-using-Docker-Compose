# Install-Jenkins-using-Docker-Compose

Installing Jenkins directly in your OS can be tricky and expensive in terms of time and resources. You need to have Java installed in your local machine and at least 10 GB of drive space. On the other hand, using docker compose is really straightforward and offers a lot of advantages

## 1. Install Docker Compose

Run this command to download the current stable release of Docker Compose:
```
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
Apply executable permissions to the binary:
```
 sudo chmod +x /usr/local/bin/docker-compose
 ```
If the command docker-compose fails after installation, check your path. You can also create a symbolic link to /usr/bin or any other directory in your path.

```
 sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
 ```
 Test the installation.
```
 docker-compose --version
 ```
 
 ## Create docker-compose configuration

Inside your working directory create the docker-compose.yml file:

```
/docker-compose.yml

version: '3.7'
services:
  jenkins:
    image: jenkins/jenkins:lts
    privileged: true
    user: root
    ports:
      - 8081:8080
      - 50000:50000
    container_name: jenkins
    volumes:
      - ~/jenkins:/var/jenkins_home
      - /var/run/docker.sock:/var/run/docker.sock
      - /usr/local/bin/docker:/usr/local/bin/docker
    
``` 

You need to ensure the directory ~/jenkins exists:
mkdir ~/jenkins

This volume will be used to persist all your data: configurations, plugins, pipelines, passwords, etc.

The remaining two volumes allow you to use docker inside the Jenkins server (Yes, you can create docker containers inside a docker container).

Run Docker Compose

/jenkins-config

```
docker-compose up -d
```    

Firs Log in
View the generated administrator password to log in the first time.
```
docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```
