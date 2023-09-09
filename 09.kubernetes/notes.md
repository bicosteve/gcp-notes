Google Kubernetes Engine - GKE

- k8s is open source container orchestration solution.
- provides cluster management to deploy containers.
- each cluster can have different types of virtual machines.
- provide features like auto scaling, service discovery, load balancing, auto healing, etc

Managed Kubernetes Service

- minimizes the operatons with auto-repari and auto upgrade features.
- provides pods and cluster auto scaling.
- enable cloud logging and cloud monitoring
- uses container-optimized OS for running containers
- provides support for persistent disks and local ssd

Microservice Journey

1. Create a cluster - with default node pool

- use gcloud container clusters create or use gcloud console

- cluster is in gcp-k8s
- enable the k8s engine api
- there are two modes when you create a cluster;
  a) auto pilot - you delegate all pods management to gke.

  - the goal is to reduce your operational costs in running k8s clusters.
  - provides you with hands off experience since you do not have to worry about node managements

  b) standard - you configure the nodes and pods yourself.

  - you can set the disk size, number of pods, location, nodes and memory.
  - gcloud container clusters get-credentials gcp-k8s-cluster --zone southamerica-west1-b --project gcp-k8s-398512
  - this will connect to yoru cluster.
  - for k8s use gcloud container clusters
    c) log in to cloud shell
  - can also use;
    -> gcloud config set project <project-name>
    d) connect to kubernetes cluster
  - connect to project by;
    -> gcloud container clusters get-credentials gcp-k8s-cluster --zone southamerica-west1-b --project gcp-k8s-398512
  - Fetching cluster endpoint and auth data.
  - kubeconfig entry generated for gcp-k8s-cluster.
    -> returns the above fetching cluster endpoint, auth data and kubeconfig entry generations

2. Deploy a microservice to k8s

- create deployment & service using kubectl commands
- kubectl create deployment <name-of-deployment> --image=<image-name>
- kubectl create deployment hello-world-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
- kubectl create deployment k8s-rest-api --image=in28min/hello-world-rest-api:0.0.1.RELEASE
  -> above will create a deployment to the cluster
- expose deployment to the outside world
  -> kubectl expose deployment hello-world-rest-api --type=LoadBalancer --port=8070
  -> kubectl expose deployment k8s-rest-api --type=LoadBalancer --port=8070
  NB: when a deployment is exposed, we are creating a service.
  - you create a deployment to deploy a microservice.
  - you create a service to expose the microservice to the outside world.
  - curl <external_ip:8080> - should return healthy:true
  - curl <external_ip:8080/hello-world> - should return hello world
  - curl 34.176.179.85:8080/hello-world

3. Play with it
