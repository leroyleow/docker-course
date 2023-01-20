# Container Orchestration - Docker Swarm & Kubernetes

# Docker Swarms
Docker Swarm is a native clustering and scheduling tool for Docker containers. It allows you to turn a group of Docker engines into a single, virtual Docker engine. With Swarm, you can easily scale your applications, distribute them across multiple hosts, and manage them in a single place. This makes it a powerful tool for managing and orchestrating containerized applications in production environments.

## How to Use
1. Initialize a Swarm: On one of your hosts, run the command <b>'docker swarm init'</b> to initialize the Swarm and make that host the manager node.

2. Join worker nodes: On other hosts, run the command <b>'docker swarm join'</b> with the join token and manager address provided by the <b>'docker swarm init'</b> command. These hosts will become worker nodes in the Swarm.

![23](/docs/imgs/sc23.jpg)


3. Deploy services: Use the <b>'docker service create'</b> command to create a new service and deploy it to the Swarm. You can specify the number of replicas and other options for the service.

4. Scale services: Use the <b>'docker service scale'</b> command to scale the number of replicas for a service. This command must be run at the swarm manager not at the nodes

![24](/docs/imgs/sc24.jpg)

5. List and manage services: Use the <b>'docker service ls'</b> command to list all services running in the Swarm and <b>'docker service ps'</b> to see the tasks of a service.

6. Update services: Use the <b>'docker service update'</b> command to update a service with new configurations.

7. Remove services: Use the <b>'docker service rm'</b> command to remove a service from the Swarm.

8. Leave or Promote/Demote Swarm: Use the <b>'docker swarm leave'</b> command to remove a node from the swarm, <b>'docker node promote'</b> and <b>'docker node demote'</b> commands to promote and demote a manager node.

You can also use swarm-specific commands like docker node ls to see the nodes in the swarm, and docker swarm info to see the overall state of the Swarm.

# Kubernetes

Kubernetes (K8s) is an open-source container orchestration system for automating the deployment, scaling, and management of containerized applications. It was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF).

Kubernetes provides a platform-agnostic way to manage containerized applications by using a set of abstractions (such as pods, services, and replication controllers) to represent the desired state of the application. It then continuously monitors the actual state of the application and makes adjustments as necessary to ensure that it matches the desired state.

Kubernetes also provides features such as automatic load balancing, self-healing, and automatic scaling, which make it well-suited for running large and complex applications in production environments. It also has a large and active community that maintains a wide variety of add-ons, tools, and solutions.

## How to use 

1. Set up a cluster: You will need to set up a cluster of machines that will run the Kubernetes control plane and worker nodes. This can be done either by using a managed Kubernetes service, such as Google Kubernetes Engine (GKE) or Amazon Elastic Kubernetes Service (EKS), or by manually setting up a cluster using tools such as kubeadm or minikube.

2. Create a deployment: Use a Kubernetes deployment resource to define the desired state of your application, including the number of replicas, the container image to use, and any environment variables or volumes required.

3. Create a service: Use a Kubernetes service resource to define how to access your application. Services can define a stable IP and DNS name, and can also load balance traffic across multiple replicas.

4. Scale your application: Use the <b>'kubectl scale'</b> command to increase or decrease the number of replicas of your deployment.

5 Monitor and troubleshoot your application: Use tools such as <b>'kubectl describe'</b> and <b>'kubectl logs'</b> to view the status of your resources, and to troubleshoot issues.

6. Update your application: Use <b>'kubectl apply'</b> command to update the configuration of your application or <b>'kubectl rolling-update'</b> command to update the image of the container.

7. Delete resources: Use <b>'kubectl delete'</b> command to delete resources.

8. Use Kubernetes CLI: Use <b>'kubectl'</b> command line tool to interact with the Kubernetes cluster, you can use it to create, update, delete, and view the status of resources in the cluster.

9. Kubernetes is a complex system, and these are just the basics. There are many other features and concepts in Kubernetes that you can learn and use, such as ConfigMaps and Secrets, Autoscaling, and Ingress.

![sc26](/docs/imgs/sc26.jpg)

## Architecture/Components
The best representation of the Kubernetes architecture is a layered approach. The main components of a Kubernetes cluster are:

![27](/docs/imgs/sc27.jpg)

* The API server: This is the front-end for the Kubernetes control plane, and it exposes the Kubernetes API. It is responsible for validating and configuring the data sent to the cluster.

* etcd: This is a consistent and highly-available key-value store used as Kubernetes’ backing store for all cluster data.

* The controller manager: This component runs controllers that handle routine tasks in the cluster, such as replicating pods, and performing rolling updates.

* The scheduler: This component is responsible for scheduling pods on nodes based on resource requirements and constraints.

* Kubelet: This runs on each node and communicates with the control plane to ensure that containers are running as expected.

* kube-proxy: This runs on each node and manages the network routing for pods.

* Pods: Pods are the smallest and simplest unit in the Kubernetes object model, and they can contain one or more containers.

* Services: Services provide a stable IP and DNS name for pods, and can also load balance traffic across multiple replicas.

* Replication controller: Replication controllers ensure that a specified number of replicas of a pod are running at all times.

* Deployments: Deployments provide declarative updates for pods and Replication controllers.

This architecture allows for a high degree of flexibility and scalability, as the components can be separated and run on different machines to spread the load and provide redundancy.