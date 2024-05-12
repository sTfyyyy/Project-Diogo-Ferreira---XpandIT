Project Diogo Ferreira - XpandIT

First of all it is needed to put the Dockerfile and the sample.war files in the same directory.

How to Build the Docker Image through a Dockerfile

1 - The first step is to open Terminal, CMD or Powershell.
2 - Then Open the directory Folder in the your command prompt and execute the next command.
docker build -t projetodiogoferreira .

Basically the command docker build uses a Dockerfile to build a docker image
the next part -t and the [name] is basically a tag on the docker image for us to guide ourselfs
and the dot part, we are saying that the Dockerfile is in that Directory we are running the docker build command.

3 - Then we need to run the docker run command as shown in the following:
docker run -d -p 4041:4041 -t projetodiogoferreira

The docker run command is used to create and run containers from images.
The -d option tells the Docker to run the container in detached mode, meaning it runs in the background.

The -p 4041:4041 option maps port 4041 of the container to port 4041 on the host. This allows the container to be accessed through port 4041 on the host machine.

The -t option allocates a pseudo-TTY, which means it attaches the container's terminal to the host's terminal. This is commonly used for containers running interactive applications.

The name projetodiogoferreira is the name of the image that will be used to create the container. Docker will look for this image locally and use it to instantiate the container.

4 - Then acess the https://localhost:4041/sample/ link to access the Tomcat's Sample web app.

Order of the commands are:
==============================
docker build -t projetodiogoferreira .
docker run -d -p 4041:4041 -t projetodiogoferreira
https://localhost:4041/sample/
==============================
