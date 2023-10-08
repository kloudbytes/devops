
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

### Overall files and directory tree structure: 
![dockerfile treee](https://github.com/kloudbytes/certified-kubernetes-administator/blob/main/images/dockerfile-tree.png)

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
      <p>Welcome to CKA & Devops world</p>
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
RUN  apt-get -y update && apt-get -y install nginx && apt-get -y install curl
COPY files/default /etc/nginx/sites-available/default
COPY files/index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["/usr/sbin/nginx", "-g", "daemon off;"]
```

### Step-3: Build Docker Image & run it

`docker build -t <your-docker-hub-id>/<container-name>:<version>`

#### Replace your docker hub account Id

```
docker build -t kloudbytes/mynginx-image:v1 .
```

### Step-4: Pushing Docker Images to a Docker Repository:

```
docker login -u kloudbytes
Password:
WARNING! Your password will be stored unencrypted in /root/.docker/config.json.
Configure a credential helper to remove this warning. See
https://docs.docker.com/engine/reference/commandline/login/#credentials-store

Login Succeeded

```
#### Check the new image:

```
docker images
REPOSITORY                   TAG       IMAGE ID       CREATED              SIZE
kloudbytes/mynginx-image     v1        a952f572c045   10 minutes ago      190MB

```

#### push the image to kloudbyetes docker hub:
```
docker push kloudbytes/mynginx-image:v1
```

```
output looks like this:

docker push kloudbytes/mynginx-image:v1
The push refers to repository [docker.io/kloudbytes/mynginx-image]
c875980d9611: Pushed
9b5262557c27: Pushed
9dd85f5ca49a: Pushing [====================>                              ]  48.52MB/117MB
954c82bdeb5f: Mounted from library/ubuntu

```

output looks like this:
```
The push refers to repository [docker.io/kloudbytes/mynginx-image]
4e4f2cd4aab5: Pushing [========>                                          ]  9.861MB/60.94MB
d26d4f0eb474: Mounted from library/nginx
a7e2a768c198: Mounted from library/nginx
9c6261b5d198: Mounted from library/nginx
ea43d4f82a03: Mounted from library/nginx
1dc45c680d0f: Mounted from library/nginx
eb7e3384f0ab: Mounted from library/nginx
d310e774110a: Mounted from library/nginx
...
...
1.0: digest: sha256:e63264f2d292c632a3615d6298706734748c2f65eeda80117a2993d96282a4e8 size: 1990
```

###  Step-5: Verify the same on the docker hub

Login to docker hub and verify the image we have pushed

Url: https://hub.docker.com/repositories

### Step-6:  run the 'mynginx-image:v1' container

```
docker run  kloudbytes/mynginx-image:v1
```

(OR)

```
#vi mynginx-1.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: mynginx-1
  name: mynginx-1
spec:
  containers:
  - image: kloudbytes/mynginx-image:v1
    name: mynginx-1
```

```
kubectl create -f mynginx-1.yaml

output:
 pod/mynginx-1 created


kubectl get po mynginx-1

outuput:
NAME        READY   STATUS    RESTARTS   AGE
mynginx-1   1/1     Running   0          55s

```
```
kubectl describe po mynginx-1 | grep image
```
output:

    Image:          kloudbytes/mynginx-image:v1

    Image ID:       docker.io/kloudbytes/mynginx-image@sha256:215950cbacefc4b59bf39f3287804f393bfeceb918e13f550c5f3b08694df78d

#### Reference:
* https://www.docker.com/
* https://docs.docker.com/
* Many Website and tutorials.
