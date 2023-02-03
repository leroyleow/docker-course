# Storing Distributing Images

## Summary

* Auto and manual build container images using Docker Hub
* Docker Hub, GitHub Packages, Microsoft's Azure Container Registry
* Own local Docker Registry
* MicroBadger, a service that allows us to display information about our remotely hosted container images.
* Secure container images distribution
* trigger an update of image with a single build

## Creating an automated build

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