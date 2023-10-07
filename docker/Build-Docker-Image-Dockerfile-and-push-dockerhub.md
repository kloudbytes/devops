
# Create a new nginx Docker Image, Run it as a Container and Push it to Docker Hub:

### Pre-requisite Steps
1.  docker software installed  machine.
2.  Create your Docker hub account.
3.  https://hub.docker.com/

The following table contains the important Dockerfile instructions and their explanation.

|Dockerfile - Instruction |	Explanation|
--------------------------|-----------|
|FROM|	To specify the base image that can be pulled from a container registry( Docker hub, GCR, Quay, ECR, etc.)|
|RUN|	Executes commands during the image build process.|
|ENV|	Sets environment variables inside the image. It will be available during build time as well as in a running container. If you want to set only build-time variables, use ARG instruction.|
|COPY|	Copies local files and directories to the image|
|EXPOSE|	Specifies the port to be exposed for the Docker container.
|ADD|	It is a more feature-rich version of the COPY instruction. It also allows copying from the URL that is the source and tar file auto-extraction into the image. However, usage of COPY command is recommended over ADD. If you want to download remote files, use curl or get using RUN.|
|WORKDIR|	Sets the current working directory. You can reuse this instruction in a Dockerfile to set a different working directory. If you set WORKDIR, instructions like RUN, CMD, ADD, COPY, or ENTRYPOINT gets executed in that directory.|
|VOLUME|	It is used to create or mount the volume to the Docker container|
|USER|	Sets the user name and UID when running the container. You can use this instruction to set a non-root user of the container.|
|LABEL|	It is used to specify metadata information of Docker image|
|ARG|	Is used to set build-time variables with key and value. the ARG variables will not be available when the container is running. If you want to persist a variable on a running container, use ENV.|
|CMD|	It is used to execute a command in a running container. There can be only one CMD, if multiple CMDs then it only applies to the last one. It can be overridden from the Docker CLI.|
|ENTRYPOINT|	Specifies the commands that will execute when the Docker container starts. If you don’t specify any ENTRYPOINT, it defaults to /bin/sh -c. You can also override ENTRYPOINT using the --entrypoint flag using CLI. Please refer CMD vs ENTRYPOINT for more information.|


### Step-1: Create customized index.html and default file:

When you build a docker image for real-time projects, it contains code or application config files.

For demo purposes, we will create a simple HTML file & config file as our app code and package it using Docker. This is a simple index.html file. You can create your own if you want.

```
mkdir nginx-docker-image ; cd nginx-docker-image

mkdir files ; cd files
```

Create a .dockerignore file
```
touch .dockerignore
```

Create a index.html file in `files` directory

vi index.html
```
<!DOCTYPE html>
<html>
   <body style="background-color:Bisque;">
      <h1>Welcome to container application...!</h1>
      <p>Welcome to CKA Course</p>
      <p>Application Version: V1</p>
   </body>
</html>
```

Create a file name default file in `files` direcotry 

```
vi default
```

Copy the following contents to the default file.
```
server {
    listen 80 default_server;
    listen [::]:80 default_server;
    
    root /usr/share/nginx/html;
    index index.html index.htm;

    server_name _;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

## Step-2: Create the Dockerfile:

### Create the Dockerfile

> Note:  Create a Dockerfile in the nginx-docker-image folder.

Here’s the simple Dockerfile content for our use case. Add the content to the Dockerfile.
vi Dockerfile
```
FROM ubuntu:20.04  
LABEL maintainer="support@kloudbytes.in"
RUN  apt-get -y update && apt-get -y install nginx && apt-get install curl
COPY files/default /etc/nginx/sites-available/default
COPY files/index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
```

### Step-3: Build Docker Image & run it

`docker build -t <your-docker-hub-id>/<container-name>:<version>`

`docker run --name <container-name> -p 80:80 -d <your-docker-hub-id>/mynginx-image:v1`

#### Replace your docker hub account Id
```
docker build -t kloudbytes/mynginx-image:v1 .
docker run --name mynginx -p 80:80 -d kloudbytesy/mynginx-image:v1
```

### Step-4: push the Docker image to Docker hub

> docker push <your-docker-hub-id>/<your-image-name>:v1

### Replace your docker hub account Id

````
docker images
docker push kloudbytes/mynginx-image:v1

````

###  Step-5: Verify the same on the docker hub

Login to docker hub and verify the image we have pushed

Url: https://hub.docker.com/repositories


#### Reference:
* https://www.docker.com/
* https://docs.docker.com/
* Many Website and tutorials.
