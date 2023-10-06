# Update the nginx image and push it to docker hub

### Run nginx image
```
docker run -it nginx
```
output looks like this: 

Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
a803e7c4b030: Pull complete
8b625c47d697: Pull complete
4d3239651a63: Pull complete
0f816efa513d: Pull complete
...
...

Login to nginx container:

```
Note: 3f4e1477e4e0 is nginx container id

docker exec -it 3f4e1477e4e0 bash
root@3f4e1477e4e0:/#
```
Check nginx web server is working or not:

```
root@3f4e1477e4e0:/# curl localhost
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>

```

Update the nginx web page:

Before update the page: install apt-get update ; apt-get install vim

```
vim /usr/share/nginx/html/index.html

update the below line in that file:

<h1>Welcome to my First Container Application...!</h1>

:wq

```
#### Docker build after updating the web content: 

```
Note:  3f4e1477e4e0 is nginx container id
-m = message
-a = author
<repository/<new-imge name:version>

docker commit -m nginx-kb-1c-app -a kloudbytes 3f4e1477e4e0 kloudbytes/nginx-kb-1c-app:1.0
```
> sha256:21bb5d6ab43b9318b2149917a76a257a3700535a246f62af1aa6174e3e1ade71

```
root@worker3:~# docker images
REPOSITORY                   TAG       IMAGE ID       CREATED         SIZE
kloudbytes/nginx-kb-1c-app   1.0       21bb5d6ab43b   4 seconds ago   248MB
```

### Pushing Docker Images to a Docker Repository:

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
REPOSITORY                   TAG       IMAGE ID       CREATED         SIZE
kloudbytes/nginx-kb-1c-app   1.0       21bb5d6ab43b   6 minutes ago   248MB

```

#### push the image to kloudbyetes docker hub:
```
docker push kloudbytes/nginx-kb-1c-app:1.0

```

output looks like this:

The push refers to repository [docker.io/kloudbytes/nginx-kb-1c-app]
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

### Go to your docker hub and check your container image.

Then, go to your k8s cluster and try to use your new image

```
#my-nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: my-nginx-pod
  name: my-nginx-pod
spec:
  containers:
  - image: kloudbytes/nginx-kb-1c-app:1.0
    name: my-nginx-pod
  ```






