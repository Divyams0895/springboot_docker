# Docker for Spring Boot: Containerization and Deployment

When a team of developers is working and sharing the code in the same environment, it leads to conflicts in the version of the library and dependencies when pull the code to local machine in other developers. There is a solution for this. That is 'Docker'.

Docker
=======
* Containerize the application (create containers)
* Means that it includes the application code, its dependencies, and the required environment configuration
* Everyone has their own container in their system. So, no conflict occurred.
* Docker solves the problem of dependency management and incompatibilities. 

What is Docker?
-----------------

* It is an open-source platform that allows you to automate the deployment, scaling, and management of applications using containerization
* Create containers using Docker, which is portable, which means that the containers can be moved across systems and can be deployed.
image

Virtual Machines
----------------
* Act like separate computers inside your computer.
* We can use multiple OS in your system.
* Each virtual machine behaves like a separate computer.
* VMs are created and managed by virtualization software.
* Software available: VMware, VirtualBox

Docker over VMs
----------------
* Docker Engine - manage the containers
* Lightweight
* There is no guest OS
image

Docker Architecture
===================
image
Components
-------------
* Host OS-installed in our system windows, Linux, or Mac
* Docker Engine - Docker CLI, Docker API, Docker Daemon together to form the engine
  Interacts with Docker containers and Docker images with the help of the daemon.
  Then, in turn, interact with the Docker registry.
   
* Docker CLI- Command Line Interface
    - A client that allows users to interact with Docker
    - CLI interacts with the daemon to get the commands executed.
    
* Docker API- 
* Docker Daemon- Runs on the host OS and is responsible for building Docker images and managing containers
	- Listens to the user's command via the Docker CLI.
	- Also interacts with Docker API and manages all the Docker objects

* Docker containers- Running instance of Docker image
  
* Docker Image - Templates with instructions for creating something called containers.
	- Blueprint for creating a container
	- Docker images and containers are managed by a daemon.

* Docker registry- A service that store doc images
	Images can be public or private
  - pull image -> get into your system
  - push image -> send to the registry

All managed with the help of doc commands

Concepts in Docker
------------------
1. Images: Docker images are templates that define the container and its dependencies.
2. Containers: Runtime environments created from Docker images.
		* They provide an isolated and reproducible execution environment for applications.
3. Docker Engine: It is the runtime that runs and manages containers
		* Provide all necessary tools and infrastructure that you will need to build, run, and manage.
4. Dockerfile: It is a file that contains instructions to build a Docker image.
* Specifies the base image
* Dependencies include and instructions

5. Docker Hub: It is a cloud-based registry that hosts a vast collection of Docker images.
           * Used to share, access, and download public images, and also provide a platform for storing and sharing your own images.

Docker Registry
--------------
It is a storage and distribution system for named Docker images.
Store images with different versions.

**Importance of Docker Registry**

1. It acts like a centralized resource for all Docker images.
2. Allows easy versioning
3. Share your Docker images

Example: There are popular databases like PostgreSQL and MongoDB that are available on Docker. They have their official images on Docker. Anyone can use the database, and if they have installed Docker on their system, they can use the image and create a container out of that image on their local system. And that will give them a running instance of the database without installing the database on their system.
We just pull the container and run the container.

**Different registries available**

1. Hub.docker.com (popular one)
2. Aws.amazon.com
3. Cloud.google.com/artifact-registry
4. Azure.microsoft.com/en-in/products/container-registry
5. Quay.io (provided by Red Hat)

Docker and Spring Boot
----------------------
1. We need a Dockerfile like below:


 


* **FROM openjdk:11**
    - It tells Docker to use the official OpenJDK 11 image from Docker Hub
* **VOLUME /tmp**
	- It creates a mount point with the specified path, and it marks it as holding externally mounted volumes.
* **ADD target/my-app.jar my-app.jar**
    - Assume the file is ready as a jar file
* **EXPOSE 8080**
    - The application will be running inside the container; we need a way to access our application.
* **ENTRYPOINT [“java”,” -jar”, “/my-app.jar”]**
    - Docker command to run when the container starts.

How Docker works on Spring Boot
--------------------------------
* Check whether the plugin ‘spring-boot-maven-plugin’ is available in the pom.xml file.

**How does the process work?**
* Cloud Native Buildpacks:
    - It is the specification created by the Cloud Native Computing Foundation, which is CNCF.
    - This is the foundation that has created this specification
    - It is designed to create container images from the source code without the need for a Docker file.
    - The buildpacks will analyse the project entirely, and they will choose the right tools and libraries for compiling it.
* Spring Boot Maven Plugin:
    - It is configured and which triggers the cloud buildpacks.
    - It packages the entire application into a jar file first, and it hands it off to the buildpacks, which will then take it from there.
* Layering:
    - Create separate parts for docker image
    - For example: one layer for JVM, another layer can contain application dependencies, and another layer may be your application.
    - Layering technique speeds up the process.
* Paketo Buildpacks:
    - Support for Spring Boot applications.
    - These are implementations of cloud native Buildpacks specification.
    - Provide JVM and other tools required to compile a Spring Boot application into a Docker image.
* Result:
    - This Docker image can run in any Docker-compatible runtime

**Advantages**

* No Dockerfile needed
* Sensible defaults
* Consistent environment
* Security
* Layering and efficiency
* Ease of use


Working with Docker Hub
1.	Create an account on https://hub.docker.com/
2.	Use the command below
  * ./mvnw spring-boot:build-image "-Dspring-boot.build-image.imageName=<your docker-username>/<docker-image-name>"
  * In Windows: mvnw spring-boot:build-image "-Dspring-boot.build-image.imageName=<your docker-username>/<docker-image-name>"
    - Before executing this command, make sure your Docker Desktop is running
3.	To pull the image on Docker:
  * In cmd: docker login
  * Using the URL and one-time device confirmation code login succeeded.
4.	In cmd: docker push image_name( <your docker-username>/<docker-image-name>) 
  * After pushing successfully, you can check your Docker Hub repository

Docker Commands
==================
1.	docker pull <image>: pull the image from docker repository
2.	docker push <username/image>: push the docker image to docker hub.
3.	docker run -it -d -p <host-port>:<container-port> --name <name> <image>: Used to run docker container from the image.
    * -it: interactive
    * -d: detached mode
    * -p: port mapping
4.	docker stop <container-id/container-name>: stop the running container
5.	docker start <container-id/container-name>: start the container
6.	docker rm <container-id/container-name>: If you want to delete a container that has stopped, you can make use of rm
7.	docker rmi <image-id/image-name>: Remove an image from local storage
8.	docker ps: Get running Docker containers.
9.	docker ps -a: Show all the containers, which are stopped as well as running
10.	docker images: List all the images on the host.
11.	docker exec -it <container-name/container-id> bash: Enables access to a running container.
12.	docker build -t <username/image> .: Used to build an image from the docker file.
13.	docker logs <container-name/container-id>: Get the logs of a particular container that is running or the container that exists.
14.	docker inspect <container-name/container-id>: Gives the detailed information of a particular container.


Running a Spring Boot project in a Docker container
---------------------------------------------------
* Before working with Docker, make sure your docker desktop up and  running
* Use the commands in the terminal:
  - docker images
  - docker ps
  - docker run -p 8080:8080 dockerdiv2025/ecom-app (use your docker-username/image-name)
  - After checking, to stop running, use Ctrl+C
  - docker run -d -p 8080:8080 dockerdiv2025/ecom-app
  - docker ps (shows the running containers)
  - docker stop container-id (stop running container)











