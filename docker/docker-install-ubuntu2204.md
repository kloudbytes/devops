# Introduction
Docker is an application that simplifies the process of managing application processes in containers. Containers let you run your applications in resource-isolated processes. They’re similar to virtual machines, but containers are more portable, more resource-friendly, and more dependent on the host operating system.

For a detailed introduction to the different components of a Docker container, check out The Docker Ecosystem: An Introduction to Common Components.

In this tutorial, you’ll install and use Docker Community Edition (CE) on Ubuntu 22.04. You’ll install Docker itself, work with containers and images, and push an image to a Docker Repository.

## Prerequisites
To follow this tutorial, you will need the following:

One Ubuntu 22.04 server is set up by following the Ubuntu 22.04 initial server setup guide, including a sudo non-root user and a firewall.

### Step 1 — Installing Docker
The Docker installation package available in the official Ubuntu repository may not be the latest version. To ensure we get the latest version, we’ll install Docker from the official Docker repository. To do that, we’ll add a new package source, add the GPG key from Docker to ensure the downloads are valid, and then install the package.

First, update your existing list of packages:
```
sudo apt update
```
Next, install a few prerequisite packages that let apt use packages over HTTPS:

```
sudo apt install apt-transport-https ca-certificates curl software-properties-common
```

Then add the GPG key for the official Docker repository to your system:

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Add the Docker repository to APT sources:

```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
Update your existing list of packages again for the addition to be recognized:

```
sudo apt update
```

Make sure you are about to install from the Docker repo instead of the default Ubuntu repo:
```
apt-cache policy docker-ce
```
You’ll see output like this, although the version number for Docker may be different:

Output of apt-cache policy docker-ce
docker-ce:
  Installed: (none)
  Candidate: 5:20.10.14~3-0~ubuntu-jammy
  Version table:
     5:20.10.14~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
     5:20.10.13~3-0~ubuntu-jammy 500
        500 https://download.docker.com/linux/ubuntu jammy/stable amd64 Packages
        
Notice that docker-ce is not installed, but the candidate for installation is from the Docker repository for Ubuntu 22.04 (jammy).

Finally, install Docker:
```
sudo apt install docker-ce
Docker should now be installed, the daemon started, and the process enabled to start on boot. Check that it’s running:
```
```
sudo systemctl status docker
```
> The output should be similar to the following, showing that the service is active and running:

Output

> ● docker.service - Docker Application Container Engine
>     Loaded: loaded (/lib/systemd/system/docker.service; enabled; vendor preset: enabled)
>     Active: active (running) since Thu 2023-10-05 06:54:32 UTC; 6h ago
> TriggeredBy: ● docker.socket
>       Docs: https://docs.docker.com
>   Main PID: 10979 (dockerd)
>      Tasks: 18
>     Memory: 225.2M
>        CPU: 18.602s
>     CGroup: /system.slice/docker.service
>             ├─10979 /usr/bin/dockerd -H fd:// --containerd=/run/containerd/containerd.sock
>             └─15449 /usr/bin/docker-proxy -proto tcp -host-ip 0.0.0.0 -host-port 80 -container-ip 172.17.0.2 -container-port 80


Installing Docker now gives you not just the Docker service (daemon) but also the docker command line utility, or the Docker client. We’ll explore how to use the docker command later in this tutorial.


