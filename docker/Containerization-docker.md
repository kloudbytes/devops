# Containerization

Containerization is a lightweight, portable, and efficient method of packaging, distributing, and running software applications and their dependencies in a consistent and isolated environment. In containerization, applications and their dependencies are bundled together in a standardized unit called a "container."

In another way, will define:

## What is containerization? 

A containerization is a form of virtualization where applications run in isolated user spaces, called containers, while using the same shared operating system (OS). One of the benefits of containerization is that a container is essentially a fully packaged and portable computing environment. Everything an application needs to run — its binaries, libraries, configuration files, and dependencies — is encapsulated and isolated in its container. 

The container itself is abstracted away from the host OS, with only limited access to underlying resources — much like a lightweight virtual machine (VM). As a result, the containerized application can be run on various types of infrastructure — on bare metal, within VMs, and in the cloud — without needing to refactor it for each environment.

## How does containerization work?

Each container is an executable package of software, running on top of a host OS. A host may support many containers (tens, hundreds, or even thousands) concurrently, such as in the case of a complex microservices architecture that uses numerous containerized application delivery controllers (ADCs). This setup works because all containers run minimal, resource-isolated processes that others cannot access.

Think of a containerized application as the top layer of a multi-tier cake:

*   At the bottom, there’s the hardware of the infrastructure in question, including its CPU(s), disk storage, and network interfaces.
*   Above that is the host OS and its kernel—the latter serves as a bridge between the software of the OS and the hardware of the underlying system.
*   The container engine and its minimal guest OS, which are particular to the containerization technology being used, sit atop the host OS.
*  At the very top are the binaries and libraries (bins/libs) for each application and the apps themselves, running in their isolated user spaces (containers).

Containerization as we know it evolved from cgroups, a feature for isolating and controlling resource usage (e.g., how much CPU and RAM and how many threads a given process can access) within the Linux kernel. Cgroups became Linux containers (LXC), with more advanced features for namespace isolation of components, such as routing tables and file systems. An LXC container can mount a file system, run commands as root, and obtain an IP address.

It performs these actions in its own private user space. While it includes the special bins/libs for each application, an LXC container does not package up the OS kernel or any hardware, meaning it’s very lightweight and can be run in large numbers even on relatively limited machines.


LXC serves as the basis for Docker, which launched in 2013 and quickly became the most popular container technology—effectively an industry standard, although the specifications set by the Open Container Initiative (OCI) have since become central to containerization. Docker is a contributor to the OCI specs, which specify standards for the image formats and runtimes that container engines use.

Someone booting a container, Docker or otherwise, can expect an identical experience regardless of the computing environment. The same set of containers can be run and scaled whether the user is on a Linux distribution or even Microsoft Windows. This cross-platform compatibility is essential to today’s digital workspaces, where workers rely on multiple devices, operating systems, and interfaces to get things done.

## What are the main benefits of containerization?

There are many benefits of containerization. Containerized apps can be readily delivered to users in a virtual workspace. More specifically, containerizing a microservices-based application, ADCs, or database (among other possibilities) offers a broad spectrum of distinctive benefits, ranging from superior agility during software development to easier cost controls.

#### Containerization technology: More agile DevOps-oriented software development 
Compared to VMs, containers are simpler to set up, whether a team is using a UNIX-like OS or Windows. The necessary developer tools are universal and easy to use, allowing for the quick development, packaging, and deployment of containerized applications across OSes. DevOps engineers and teams can (and do) leverage containerization technologies to accelerate their workflows.

#### Less overhead and lower costs than virtual machines
A container doesn’t require a full guest OS or a hypervisor. That reduced overhead translates into more than just faster boot times, smaller memory footprints and generally better performance, though. It also helps trim costs, since organizations can reduce some of their server and licensing costs, which would have otherwise gone toward supporting a heavier deployment of multiple VMs. In this way, containers enable greater server efficiency and cost-effectiveness.

#### Fault isolation for applications and microservices
If one container fails, others sharing the OS kernel are not affected, thanks to the user space isolation between them. That benefits microservices-based applications, in which potentially many different components support a larger program. Microservices within specific containers can be repaired, redeployed, and scaled without causing downtime of the application.

#### Easier management through orchestration
Container orchestration via a solution such as Kubernetes platform makes it practical to manage containerized apps and services at scale. Using Kubernetes, it’s possible to automate rollouts and rollbacks, orchestrate storage systems, perform load balancing, and restart any failing containers. Kubernetes is compatible with many container engines including Docker and OCI-compliant ones.

#### Excellent portability across digital workspaces
Another one of the benefits of containerization is that containers make the ideal of “write once, run anywhere” a reality. Each container is abstracted from the host OS and runs the same in any location. As such, it can be written for one host environment and then ported and deployed to another, as long as the new host supports the container technologies and OSes in question. Linux containers account for a big share of all deployed containers and can be ported across different Linux-based OSes whether they’re on-premises or in the cloud. On Windows, Linux containers can be reliably run inside a Linux VM or through Hyper-V isolation. Such compatibility supports digital workspaces where numerous clouds, devices, and workflows intersect.


## containerization software list:

While Docker is one of the most popular containerization platforms, there are several other containerization solutions available in the market. Here are some notable containerization platforms similar to Docker:

#### Podman:
Podman is an open-source containerization tool that aims to provide an experience similar to Docker but with additional features like rootless containers, pod-based orchestration (similar to Kubernetes pods), and compatibility with Docker commands and images.

#### rkt (pronounced "rocket"):
rkt is an open-source containerization solution developed by CoreOS. It is known for its focus on simplicity, security, and composability. rkt is designed to work well with other tools and standards.

#### containerd:
containerd is an industry-standard container runtime that provides the basic functionalities needed to run containers. It's used as the container runtime for Docker and Kubernetes, providing a simple and stable platform for running containers.

cri-o:
cri-o is a lightweight container runtime specifically built for Kubernetes. It's optimized for Kubernetes use cases and aims to provide a simpler and more specialized alternative to Docker for running containers in Kubernetes clusters.

#### OpenShift:
OpenShift is an open-source containerization and Kubernetes-based platform developed by Red Hat. It provides a complete solution for building, deploying, and managing applications in containers. It includes features such as automated scaling, CI/CD pipelines, and developer and operational tools.

#### LXC (Linux Containers):
LXC is an operating system-level virtualization method that allows running multiple Linux distributions on a single host. While it predates Docker, it's similar in the sense that it provides lightweight, portable, and isolated environments.

and etc., 

These tools offer various features and functionalities, so the choice of which to use depends on your specific requirements, preferences, and the ecosystem you are working within.



