# Managing Containers

## Interacting with your containers

So far, our containers have been running a single process. Docker provides you with a few tools that enable you to both fork additional processes and interact with them, and we will be covering those tools in the following sections:

### attach

```
$ docker container attach nginx-test
```

### exec
The attach command is useful if you need to connect to the process your container is running, but what if you need something that is a little more interactive?

You can use the exec command. This spawns a second process within the container that you can interact with. For example, to see the contents of the /etc/debian_version file, we can run the following command:

```
$ docker container exec nginx-test cat /etc/debian_version
<another interactive example>
$ docker container exec -i -t nginx-test /bin/bash
```

The -i flag is shorthand for --interactive, which instructs Docker to keep stdin open so that we can send commands to the process. The -t flag is short for –tty and allocates a pseudo-TTY to the session.

### logs
logs allows you to interact with the stdout stream of your containers, which Docker is keeping track of in the background. For example, to view the last entries written to stdout for our nginx-test container, you just need to use the following command:
```
$ docker container logs --tail 5 nginx-test
<view logs in realtime>
$ docker container logs -f nginx-test
<another example>
$ docker container logs --since 2020-03-28T15:52:00 -t nginx-test
```

### stats
The stats command provides real-time information on either the specified container including CPU, RAM, NETWORK, DISK, IO and PIDS.
![masterdocker8](/docs/imgs/masterdocker8.JPG)

The stats command we ran showed us the resource utilization of our containers. By default, when launched, a container will be allowed to consume all the available resources on the host machine if it so requires. We can put limits on the resources our containers can consume.

The stats command we ran showed us the resource utilization of our containers. By default, when launched, a container will be allowed to consume all the available resources on the host machine if it so requires. We can put limits on the resources our containers can consume.

E.g. halve the CPU priority and set a memory limit of 128M, we would have used the following command:
```
$ docker container run -d --name nginx-test --cpu-shares 512 --memory 128M -p 8080:80 nginx
```
If container is already running ,use update command:-
```
$ docker container update --cpu-shares 512 --memory 128M nginx-test
```
Running above will cause memoryswap limit error
```
Error response from daemon: Cannot update container 662b6e5153ac77685f25a1189922d7f49c2df6b2375b3635a37eea 4c8698aac2: Memory limit should be smaller than already set memoryswap limit, update the memoryswap at the same time
```
So, what is the memoryswap limit currently set to? To find this out, we can use the inspect command to display all of the configuration data for our running container; just run the following command and look for lines containing memory:
```
$ docker container inspect nginx-test | grep -i memory
```
This returns the following configuration data:
```
            "Memory": 0,
            "KernelMemory": 0,
            "KernelMemoryTCP": 0,
            "MemoryReservation": 0,
            "MemorySwap": 0,
            "MemorySwappiness": null,
```
Everything is set to 0, so how can 128M be smaller than 0? In the context of the configuration of the resources, 0 is actually the default value and means that there are no limits. 
Solution 
```
$ docker container update --cpu-shares 512 --memory 128M --memory-swap 256M nginx-test
```

## Container states
docker container states command -> pause, unpause, stop, start, restart, kill
```
$ docker container stop nginx2
```

With this, a request is sent to the process for it to terminate, called SIGTERM. If the process has not terminated itself within a grace period, then a kill signal, called SIGKILL, is sent. This will immediately terminate the process, not giving it any time to finish whatever is causing the delay; for example, committing the results of a database query to disk.

Because this could be bad, Docker gives you the option of overriding the default grace period, which is 10 seconds, by using the -t flag; this is short for --time. For example, running the following command will wait up to 60 seconds before sending a SIGKILL command, in the event that it needs to be sent to kill the process:
```
$ docker container stop -t 60 nginx3
```
To remove exited containers, use prune command:
```
$ docker container prune
```
The next command we are going to quickly look at is the port command; this displays the port number along with any port mappings for the container:
```
$ docker container port nginx-test
$ docker container cp nginx-test:/tmp/testing testing -> copy out file
<copy in file>
$ echo "This is a test of copying a file from the host machine to the container" > testing
$ docker container cp testing nginx-test:/tmp/testing
$ docker container exec nginx-test cat /tmp/testing
```
![masterdocker9](/docs/imgs/masterdocker9.JPG)

