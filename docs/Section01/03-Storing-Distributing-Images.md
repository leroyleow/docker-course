# Storing Distributing Images

## Summary

* Auto and manual build container images using Docker Hub
* Docker Hub, GitHub Packages, Microsoft's Azure Container Registry
* Own local Docker Registry
* MicroBadger, a service that allows us to display information about our remotely hosted container images.
* Secure container images distribution
* trigger an update of image with a single build

## Creating an automated build
In this section, we will look at automatied builds where you can link to your Github / Bitbucket account(s). As you update the code in your code repository, you can have the image automatically built on Docker Hub.

### Setting up your code
Setup code repo in either Github or Bitbucket.

### Setting up Docker Hub
In Docker Hub, we are going to use the Create Repository button, which can be found under Repositories. After clicking it, we will be taken to a screen where we need to provide a little information about our build. We will also need to select a source:
![masterdock1.jpg](/docs/imgs/masterdocker2.JPG)

Example Information filled:-
* Repository Namespace and Name: dockerfile-example
* Short Description: Testing an automated build
* Visibility: Public

Under Build Setting, select Github

* Organization: Select the Github org  where your code is hosted
* Repository: code repo

Click + icno next to Build Rule and enter following:-
* Source Type: Branch
* Source: master
* Docker Tag: latest
* Dockerfile Location: Dockerfile
* Build Caching: Leave this selected

Click Create

### When connecting Docker Hub to GitHub, there are two options:

> * <b>Public</b>: If you link your accounts using this option, Docker Hub won't be able to configure the web hooks needed for automated builds. You then need to search and select the repository from either of the locations you want to create the automated build from. This will essentially create a web hook that is triggered each time a commit is made on the selected GitHub repository. With this, a new build will be created on Docker Hub.
> * <b>Private</b>: his is the recommended option out of the two as Docker Hub will have access to all your public and private repositories, as well as organizations. Docker Hub will also be able to configure the web hooks needed when setting up automated builds.

![masterdocker3.jpg](/docs/imgs/masterdocker3.JPG)

We can add additional configuration by clicking on <b>Build</b>. Since we are using the official Alpine Linux image in our Dockerfile, we can link that to our own build. Follow these steps:
1. Click on the Configure Automated Builds button. Then, click on the Enable for Base Image radio icon in the Repository Links section of the configuration and then the Save button.
2. This will kick off an unattended build each time a new version of the official Alpine Linux image is published.
3. Next, scroll down to Build Rules. For the Build Context setting, enter ./chapter02/dockerfile-example/. This will make sure that Docker's build servers can find any files that we add to our Dockerfile:
![masterdocker4.jpg](/docs/imgs/masterdocker4.JPG)
So, now, our image will be automatically rebuilt and published whenever we update the GitHub repository, or when a new official image is published.

As neither of these is likely to happen immediately, click on the Trigger button on the Builds page to manually kick off a build. Once you have triggered your build, scroll down to Recent Builds. This will list all of the builds for the image – successful, failed, and ones that are in progress. 

Once built, you should then able to move to your local Docker installation by running the following commands:- 
```
$ docker image pull masteringdockerfouthedition/dockerfile-example:latest
$ docker image ls
<OR>
$ docker container run -d -p8080:80 --name example masteringdockerthirdedition/dockerfiles-example
```

## Pushing you own image

To do this, we frist need to link our Docker client to Docker Hub by running the following command: 
```
$ docker login
```
If have enable 2FA, then will need to use a personal access token rather than password to login. To create a personal access token, go to Settings in Docker Hub, click on Security -> New Access Token. 

Next build and push the image to docker hub with following command:
```
$ docker build --tag alck01/dockertest:latest .
$ docker image push alck01/dockertest:latest
```

# Deploying your own Docker Registry
Docker Registry is an open source application that you can run anywhere you please and store your Docker image in. Docker Registry makes a lot of sense if you want to deploy your won registry without having to pay for all the private features of Docker Hub. 

To create own registry, run following:-
```
$ docker image pull registry:2
$ docker container run -d -p 5000:5000 --name registry registry:2
```
Next we look how we can push and pull an image to it .
```
$ docker image pull alpine
```
Now we have a copy of Alpine Linux image, we push to local Docker Registry which is available at localhost:5000
```
$ docker image tag alpine localhost:5000/localalpine
$ docker image push localhost:5000/localalpine
```
Run the following with list 2 images with the same IMAGE ID:
```
docker image ls
```
![masterdocker5](/docs/imgs/masterdocker5.JPG)

Before we pull the image back down from our local Docker registry, we should remove the 2 local copies of the image. We need to use the REPOSITORY name to do this, rather than IMAGE ID, since we have 2 images from 2 location with the same ID, Docker will throw error
```
docker image rm alpine localhost:5000/localalpine
docker image ls
```

# Reviewing 3rd Party Registries

## GitHub Package and Actions
First of all, we are going to need a personal access token. To get this, log into your GitHub account and go to Settings, then Developer settings, and then Personal access tokens.

Generate an access token, call it cli-package-access, and give it the following permissions:
* repo
* write:package
* read:package
* delete:package
* workflow

After this, put the token in a file called .githubpackage
```
$ cat ~/.githubpackage | docker login docker.pkg.github.com -u russmckendrick --password-stdin
<Build image>
$ docker image build --tag docker.pkg.github.com/russmckendrick/mastering-docker-fourth-edition/dockerfile-example:latest .
<Once built & tagged, push image>
$ docker image push docker.pkg.github.com/russmckendrick/
mastering-docker-fourth-edition/dockerfile-example:latest
```
Once pushed, you should be able to see that there is a now a package in your repository:- 
![masterdocker6](/docs/imgs/masterdocker6.JPG)

You can download the image running following:-
```
$ docker image pull docker.pkg.github.com/russmckendrick/
mastering-docker-fourth-edition/dockerfile-example:latest
```
### Create A Github Actions
To create a GitHub Action, go to your repository and click on the Actions tab. Then, click on the New workflow button. This will default to a list of published Actions. Here, just click on the Set up a workflow yourself button; this will create a file called .github/workflows/main.yml in your repository.
```
name: Create Multi Stage Docker Image CI
on: [push]
jobs:
  build_docker_image:
    runs-on: ubuntu-18.04
    steps:
    - uses: actions/checkout@v1
    - name: Build and tag image
      run: docker build -t "docker.pkg.github.com/$(echo $GITHUB_REPOSITORY | tr '[:upper:]' '[:lower:]')/multi-stage:$GITHUB_RUN_NUMBER" -f ./chapter02/multi-stage/Dockerfile ./chapter02/multi-stage/
    - name: Docker login
      run: docker login docker.pkg.github.com -u $GITHUB_ACTOR -p $GITHUB_TOKEN
      env:
        GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
    - name: Publish to GPR
      run: docker push "docker.pkg.github.com/$(echo $GITHUB_REPOSITORY | tr '[:upper:]' '[:lower:]')/multi-stage:$GITHUB_RUN_NUMBER"
```

## Azure Container Registry

Once logged in, type Container registries into the search bar at the top of the screen and select the option from the results. Once the Container registries page loads, click on the + Add button.

* Subscription
* Resource Group
* Registry Name: Choose a unique name for registry
* Location
* Admin User
* SKU

We are going to ignore the encryption options for now as they are only available when using the premium SKU as well as the tags, so click on Review + Create. 

As with GitHub Packages, we are going to build and push a container. To do this, we need some credentials. To find these, click on Access Keys and make a note of the details for Login server, Username, and one of the two passwords:

Like with GitHub Packages, put the password in a text file. I used ~/.azureacrpassword. Then, log in with the Docker command-line client by running the following:
```
$ cat ~/.azureacrpassword | docker login masteringdocker.azurecr.io -u masteringdocker --password-stdin
$ docker image build --tag masteringdocker.azurecr.io/dockerfile-example:latest .
$ docker image push masteringdocker.azurecr.io/dockerfile-example:latest
```

Once pushed, you should be able to see it listed on the Azure Container Registry page by clicking on Registries in the Services section of the main menu. Select the image and version. After doing this, you will see something like the following:
![masterdocker7](/docs/imgs/masterdocker7.JPG)

When it comes to pulling images, you will need to make sure that you are authenticated against your Container Registry as it is a private service. Trying to pull and not being logged in will result in an error.

## Further Readings
https://azure.microsoft.com/en-gb/products/container-registry/
https://learn.microsoft.com/en-gb/azure/container-registry/container-registry-tutorial-quick-task
