# Docker Compose

Docker Compose is a tool for defining and running multi-container Docker applications. With Compose, you use a YAML file to configure your application's services. Then, with a single command, you create and start all the services from your configuration.

Using Compose is a great way to simplify your Docker workflow. You can use it to easily set up and run complex applications, and to manage all the moving parts that make up your application.

For example, suppose you have a multi-service application that includes a web server, a database, and a cache. With Compose, you can define all the services in a single YAML file, and use a single command to start all the services at once. This makes it easy to manage your application, and to scale it up or down as needed.

Overall, Docker Compose is a powerful tool that can help you build and run complex applications using Docker.

## Example 1
Here is an example of a docker-compose.yml file that defines a simple application with a web service and a database service:
```
version: '3'
services:
  web:
    build: .
    ports:
      - "5000:5000"
    volumes:
      - .:/code
    depends_on:
      - db
  db:
    image: postgres:latest
    volumes:
      - db-data:/var/lib/postgresql/data
volumes:
  db-data:
```

This docker-compose.yml file defines two services: web and db. The web service is built from the current directory (.), and it exposes port 5000 on the host. The db service is based on the postgres image, and it creates a volume to store the data.

To start these services, you can use the following command:

```
$ docker-compose up
```

This will build the web service, pull the postgres image, and start both services. You can then access the web service at http://localhost:5000.

You can also use the down command to stop and remove the services:
```
$ docker-compose down
```

## Example 2
![sc9](/docs/imgs/sc9.jpg)

## Example 3 - Sample Voting Application 

Step 1 : Use Docker Run first
Putting the below application under 1 single Docker Host. Please aside the Docker Swarms.
![sc10](/docs/imgs/sc10.JPG)

### docker run --links
The command is used to link containers. The way of linking maybe deprecated with the intro of Docker Swarms. THis is only applicable to Docker Compose version 1. not require in Version 2 and 3. Version 3 comes with Docker Swarm and Docker Stacks.
![sc11](/docs/imgs/sc11a.jpg)

Step 2 : Build yml file based on above result, Run Docker Compose using yml file.
![12](/docs/imgs/sc12.jpg)

The above approach requires the docker images to be built first. Alternatively is image not build, we can specify build parameters in yml which will look for the application Docker file to build.
![13](/docs/imgs/sc13.jpg)

## Docker compose - versions
![sc14](/docs/imgs/sc14.jpg)

### Example - Network Segegration
![sc15](/docs/imgs/sc15.jpg)

## Resources
* https://docs.docker.com/compose/
* https://docs.docker.com/compose/gettingstarted/
* https://github.com/dockersamples/example-voting-app
* https://docs.docker.com/engine/reference/commandline/compose/