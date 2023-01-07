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
docker build Dockerfile -t leroyleow/my-custom-app -> build layered architecture
docker push leroyleow/my-custom-app -> push image to docker hub
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

![sc8](/docs/imgs/sc8.jpg)

## Resources
https://devopscube.com/build-docker-image/

