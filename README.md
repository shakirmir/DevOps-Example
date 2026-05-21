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
To run Jenkins jobs that build Docker images, Jenkins itself needs the Docker CLI and access to the host Docker daemon. The cleanest setup is a custom Jenkins image plus a mounted Docker socket.

# Dockerfile for Jenkins
Use the `JenkinsDockerfile` in this repository:

```bash
docker build -f JenkinsDockerfile -t jenkins-docker .
```

Run Jenkins with the Docker socket mounted:

```bash
docker rm -f vibrant_pare
docker run -d \
  --name jenkins \
  -p 8080:8080 \
  -p 50000:50000 \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v jenkins_home:/var/jenkins_home \
  jenkins-docker
```

If the host user cannot run Docker directly, add it to the `docker` group and log back in:

```bash
sudo usermod -aG docker ubuntu
```

# Building Spring Boot Application
The Sample application built here has the maven jar plugin, so in order to build that as a jar, we just have to do the command:

$ mvn clean install

