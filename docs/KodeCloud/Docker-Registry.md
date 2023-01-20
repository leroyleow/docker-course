# Docker Registry

## Docker Public Registry

There are 2 common public docker registry :- docker.io, gcr.io. For gcr.io will more on the kubernetes images

## Deploy Private Registry [On Premis]
![sc23](/docs/imgs/sc23.jpg)

E.g. Setup registry server with requirements :- Container Name:my-registry, Host Port: 5000, Image: registry:2 , Restart Policy:always
```
docker run -d -p 5000:5000 --restart=always --name my-registry registry:2
```

To check the list of images pushed, use "curl -X GET localhost:5000/v2_catalog"