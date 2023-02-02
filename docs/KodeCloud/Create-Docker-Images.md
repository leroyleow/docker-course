# Create Docker Images

## E.g. How to create Own Images
Steps
1. OS - Ubuntu
2. Update apt repo
3. Install dependencies using apt
4. Install Python dependencies using pip
5. Copy source code to /opt folder
6. Run the web server using "flask" command

Dockerfile
```
FROM Ubuntu

RUN apt-get update
RUN apt-get install python

COPY . /opt/source-code

ENTRYPOINT FLASK_APP=/opt/source-code/app.py flask run
```
![sc4](/docs/imgs/sc4.JPG)

Build image 
```
docker build . -> build layered architecture
<OR>
docker build -t leroyleow/my-custom-app:latest . -> build for docker.io
docker push leroyleow/my-custom-app:latest -> push image to docker hub
```
![sc5](/docs/imgs/sc5.JPG)


## ENV Variables in Docker

To pass a variable to environment in container. You can run multiple docker containers.
```
docker run -e APP_COLOR=blue simple-webapp-color
```

To see what environment variable to pass in using <b>docker inspect</b> cmd. This command can inspect both docker images and containers.
![sc6](/docs/imgs/sc6.JPG)


## Note 
Docker run on linux, even using Docker Desktop for Windows. Docker Desktop for Windows run on HyperV. Window images can is support for Window Server 2016 and Window 10 Profession. Base images [windows] are "Windows Bare Metal" & "Windows Nano"

## CMD vs EntryPoint

![sc7](/docs/imgs/sc7.JPG)

Both CMD and EntryPoint should be used hand in hand as follows. Note command and entrypoint should specify as json format e.g CMD ["sleep", "5"] :-

![sc8](/docs/imgs/sc8.JPG)

The ENTRYPOINT specifies a command that will always be executed when the container starts.

The CMD specifies arguments that will be fed to the ENTRYPOINT.

If you want to make an image dedicated to a specific command you will use ENTRYPOINT ["/path/dedicated_command"]

Otherwise, if you want to make an image for general purpose, you can leave ENTRYPOINT unspecified and use CMD ["/path/dedicated_command"] as you will be able to override the setting by supplying arguments to docker run.

For example, if your Dockerfile is:
```
FROM debian:wheezy
ENTRYPOINT ["/bin/ping"]
CMD ["localhost"]
Running the image without any argument will ping the localhost:
```
```
$ docker run -it test
PING localhost (127.0.0.1): 48 data bytes
56 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.096 ms
56 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.088 ms
56 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.088 ms
^C--- localhost ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max/stddev = 0.088/0.091/0.096/0.000 ms
Now, running the image with an argument will ping the argument:
```
```
$ docker run -it test google.com
PING google.com (173.194.45.70): 48 data bytes
56 bytes from 173.194.45.70: icmp_seq=0 ttl=55 time=32.583 ms
56 bytes from 173.194.45.70: icmp_seq=2 ttl=55 time=30.327 ms
56 bytes from 173.194.45.70: icmp_seq=4 ttl=55 time=46.379 ms
^C--- google.com ping statistics ---
5 packets transmitted, 3 packets received, 40% packet loss
round-trip min/avg/max/stddev = 30.327/36.430/46.379/7.095 ms
For comparison, if your Dockerfile is:
```
```
FROM debian:wheezy
CMD ["/bin/ping", "localhost"]
Running the image without any argument will ping the localhost:
```
```
$ docker run -it test
PING localhost (127.0.0.1): 48 data bytes
56 bytes from 127.0.0.1: icmp_seq=0 ttl=64 time=0.076 ms
56 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.087 ms
56 bytes from 127.0.0.1: icmp_seq=2 ttl=64 time=0.090 ms
^C--- localhost ping statistics ---
3 packets transmitted, 3 packets received, 0% packet loss
round-trip min/avg/max/stddev = 0.076/0.084/0.090/0.000 ms
But running the image with an argument will run the argument:
```
```
docker run -it test bash
root@e8bb7249b843:/#
```

## Resources
* https://devopscube.com/build-docker-image/
* https://phoenixnap.com/kb/docker-cmd-vs-entrypoint
