# Building Container Images

## Summary
* How to create, build, run Dockerfile
* Use an existing container
* Use scratch as a base
* Use ENV in Dockerfile
* Use Multi-stage builds

## A DockerFile Example
```
FROM alpine:latest
LABEL maintainer=”Russ McKendrick <russ@mckendrick.io>”
LABEL description=”This example Dockerfile installs NGINX.”
RUN apk add --update nginx && \
    rm -rf /var/cache/apk/* && \
    mkdir -p /tmp/nginx/
COPY files/nginx.conf /etc/nginx/nginx.conf
COPY files/default.conf /etc/nginx/conf.d/default.conf
ADD files/html.tar.gz /usr/share/nginx/
EXPOSE 80/tcp
ENTRYPOINT [“nginx”]
CMD [“-g”, “daemon off;”]
```

Alpine Linux is a small, independently developed, non-commercial Linux distribution designed for security, efficiency, and ease of use. While small, it offers a solid foundation for container images due to its extensive repository of packages, and also thanks to the unofficial port of grsecurity/PaX, which is patched into its kernel. This port offers proactive protection against dozens of potential zero-day threats and other vulnerabilities.

The FROM instruction tells Docker which base you would like to use for your image. As we already mentioned, we are using Alpine Linux, so we simply have to state the name of the image and the release tag we wish to use.

The LABEL instruction can be used to add extra information to the image. This information can be anything from a version number to a description. You can view the containers’ labels with the following docker inspect command.
```
$ docker image inspect <IMAGE_ID>
```

Alternatively, you can use the following command to filter just the labels:
```
$ docker image inspect -f {{.Config.Labels}} <IMAGE_ID>
```
The RUN instruction is where we interact with our image to install software and run scripts, commands, and other tasks. the && operator to move on to the next command if the previous command was successful.

The COPY instruction is simple copy. The ADD instruction copy and uncompress the file.

The ENTRYPOINT & CMD refer to [Create-Docker-Images](/docs/KodeCloud/Create-Docker-Images.md)

The USER instruction lets you specify the username to be used when a command is run. The USER instruction can be used on the RUN instruction, the CMD instruction, or the ENTRYPOINT instruction in the Dockerfile. Also, the user defined in the USER instruction must exist, or your image will fail to build. Using the USER instruction can also introduce permission issues, not only on the container itself, but also if you mount volumes.

The WORKDIR instruction sets the working directory for the same set of instructions that the USER instruction can use (RUN, CMD, and ENTRYPOINT). It will allow you to use the CMD and ADD instructions as well.

The ONBUILD instruction lets you stash a set of commands to be used when the image is used in the future, as a base image for another container image. For example, if you want to give an image to developers and they all have a different code base that they want to test, you can use the ONBUILD instruction to lay the groundwork ahead of the fact of needing the actual code. Then, the developers will simply add their code to the directory you ask them to, and when they run a new Docker build command, it will add their code to the running image.The ONBUILD instruction can be used in conjunction with the ADD and RUN instructions, such as in the following example:
```
ONBUILD RUN apk update && apk upgrade && rm -rf /var/cache/
apk/*
```
The ENV instruction sets ENVs within the image both when it is built and when it is executed. These variables can be overridden when you launch your image.
```
ENV <key> <value>
ENV username admin
```
OR
```
ENV <key>=<value>
ENV username=admin
ENV username=admin database=wordpress tableprefix=wp
ENV PHPVERSION=7 [To change version, amend in the dockerfile]
```
## Dockerfile Best Practices
* You should try to get into the habit of using a .dockerignore file. We will cover the .dockerignore file in the Building Docker images section of this chapter; it will seem very familiar if you are used to using a .gitignore file. It will essentially ignore the items you specified in the file during the build process.
* Remember to only have one Dockerfile per folder to help you organize your containers.
* Use a version control system, such as Git, for your Dockerfile; just like any other text-based document, version control will help you move not only forward, but also backward, as necessary.
* Minimize the number of packages you need to install per image. One of the biggest goals you want to achieve while building your images is to keep them as small and secure as possible. Not installing unnecessary packages will greatly help in achieving this goal.
* Make sure there is only one application process per container. Every time you need a new application process, it is good practice to use a new container to run that application in.
* Keep things simple; over-complicating your Dockerfile will add bloat and potentially cause you issues down the line.

## Build Docker 
If the name of your file is Dockerfile then you don't need the -f and the filename. else You need to run docker build -f [docker_file_name] . (don't miss the dot at the end).
```
$ docker image build --file <path_to_Dockerfile> --tag <REPOSITORY>:<TAG> .
[OR]
docker build .
[OR]
<Note>
docker image build == docker build
```

## Resource 
https://stackify.com/docker-build-a-beginners-guide-to-building-docker-images/
