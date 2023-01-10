# Docker Engine & Docker Storage
The Docker architecture consists of a Docker daemon, a REST API, and a command-line interface (CLI).

The Docker daemon (also called the Docker engine) is a background process that runs on a host machine and manages the containers. The Docker daemon listens for API requests and manages the containers, images, and networks.

The REST API is a HTTP API that allows you to communicate with the Docker daemon from any language that can make HTTP requests. You can use the REST API to build your own tools or integrate Docker with other tools and services.

The CLI is a command-line interface for interacting with the Docker daemon. You can use the CLI to build and manage containers, images, and networks.

In addition to the Docker daemon, API, and CLI, the Docker architecture includes a number of other components, such as containerd, runC, and containerd-shim, that work together to provide the containerization and orchestration features of Docker.

You can also interact with the Docker Engine remote, as follows:-
![sc16](/docs/imgs/sc16a.jpg)

## Containerization - Namespace
In the context of containerization, a namespace is a way to isolate resources within a Linux system. A namespace provides a layer of isolation by providing a separate view of certain global system resources. There are several types of namespaces in Linux, including the process ID (PID) namespace, the network namespace, and the mount namespace.

The PID namespace is used to isolate the process tree of a container from the host system and from other containers. When a process is created within a PID namespace, it is given a PID that is unique within that namespace, but the same PID may be used by a different process in a different namespace. This allows multiple containers to run on the same host and for each container to have its own view of the process tree, with the processes in each container appearing as the "init" process (with PID 1) within that container's PID namespace.

The network namespace is used to isolate the networking stack of a container from the host system and from other containers. Each container has its own network namespace, with its own network interfaces, IP addresses, and routing tables.

The mount namespace is used to isolate the file system mount points of a container from the host system and from other containers. Each container has its own mount namespace, with its own set of mounted file systems.

Together, these namespaces allow containers to have their own isolated view of certain global system resources, such as processes, networking, and file systems, while still sharing the underlying host system resources.

## cgroups
Cgroup is a linux feature to limit, police, and account the resource usage for a set of processes. It provides mechanism to limit and monitor system resources like CPU time, system memory, disk bandwidth, network bandwidth, etc.

The cgroups works by dividing resources into groups and then assigning tasks to those groups.

Docker uses cgroups to limit the system resources.

## Docker Storage
How does Docker stored the files of an image and a container. In following example, since the 1st 3 layers are the same, Dockers can reused it. Docker files are stored in <b>/var/lib/docker</b>
![sc17](/docs/imgs/sc17.jpg)
Container layer is read / write, but data is not persist when container exit. Image layer is read only
![sc18](/docs/imgs/sc18.jpg)

Solution: Add persistence volume to container (Volume Mounting)
1. Step1 -> Create Volumn
2. Step2 -> attach volume to container. If volume not created during run, Docker will create and mount
![sc19](/docs/imgs/sc19b.jpg)

## Resources

* https://docs.docker.com/engine/
* https://docs.docker.com/engine/api/
* https://docs.docker.com/config/containers/runmetrics/#control-groups
* https://dockerlabs.collabnix.com/advanced/security/cgroups/
* https://shekhargulati.com/2019/01/03/how-docker-uses-cgroups-to-set-resource-limits/
* https://dockerlabs.collabnix.com/ -> docker labs from basic to advance
* https://docs.docker.com/storage/
* https://docs.docker.com/storage/storagedriver/



