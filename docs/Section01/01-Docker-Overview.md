# Docker Overview 

## Installing Docker on Linux (Ubuntu)
```
$ curl -sSL https://get.docker.com/ | sh
$ sudo systemctl start docker
```
You will be asked to add your current user to the Docker group. To do this, run following:
```
$ sudo usermod -aG docker <username>
```
To view docker version run following:
```
 $ docker version
```
### Install Docker Compose
```
$ COMPOSEVERSION=1.25.4
$ curl -L https://github.com/docker/compose/releases/
download/$COMPOSEVERSION/docker-compose-`uname -s`-`uname -m` 
>/tmp/docker-compose
$ chmod +x /tmp/docker-compose
$ sudo mv /tmp/docker-compose /usr/local/bin/docker-compose
```
To view docker compose version run following:
```
$ docker-compose version
```
