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

3. Play with it - DONE

4. Increasing Number of Instances of Microservices
   kubectl scale deployment k8s-rest-api --replicas=3
   kubectl get pods
   kubectl get deployments

5. Manual Scaling of the number of nodes in your Kubernetes cluster.
   can increase or decrease the number of nodes in your cluster
   gcloud container clusters resize <cluster-name> --node-pool <the-node-pool> --num-nodes=<node-number> --zone=<zone>
   gcloud container clusters resize gcp-k8s-cluster --node-pool default-pool --num-nodes=5 --zone=southamerica-west1-b

6. Auto Scaling the Microservices
   can set up auto scaling for your services
   kubectl autoscale deployment k8s-rest-api --max=4 --cpu-percent=70
   this creates horizontal pod autoscaling - HPA - kubectl get hpa

7. Set Up Auto Scaling For Your Cluster
   gcloud container clusters update cluster-name --enable-autoscaling --min-nodes=1 --max-nodes=10
   gcloud container clusters update k8s-rest-api --enable-autoscaling --min-nodes=1 --max-nodes=10
   this will increase the number of nodes in the cluster and set min nodes to be 1 and max nodes to be 10

8. Configure/Add application Configuration Microservice
   a kubernetes ConfigMap is an API object that allows you to store data as key-value pairs.
   kubernetes pods can use ConfigMaps as configuration files, environment variables or command-line arguments.
   use config map
   kubectl create configmap my-config --from-literal=DB_NAME=todos
   kubectl create configmap k8s-rest-api --from-literal=k8s-rest-api-db
   kubectl get configmap
   kubectl get configmap k8s-rest-api
   kubectl describe configmap k8s-rest-api

9. Storing Secrets like Passwords for your micro services
   kubectl create secret generic k8s-rest-api-secrets --from-literal=PASSWORD=pass1234
   kubectl get secret
   kubectl describe secret k8s-rest-api-secrets

NB: to store configurations, use config map. To store password use secret
Executing Commands is called imperative style.
Writing yaml is called declarative style.
use kubectl apply -f file.yml
to use the k8s-rest-api configs, run kubectl apply -f k8s-rest-api

10. Deploy a new microservice which needs nodes with a GPU attached.
    attach a new node pool with GPU instances to your cluster
    gcloud container node-pools create POOL_NAME --cluster CLUSTER_NAME
    gcloud container node-pools list --zone=us-central1-c --cluster=k8s-rest-api
    Deploy an new microservice to the new pool by setting up nodeSelector in the deployment.yml
    : nodeSelector: cloud.google.com/gke-nodepool:POOL_NAME

11. Delete the Microservice

    1. kubectl delete service <service-name>
    2. kubectl delete deployment <deployment-name>

12. Delete the Cluster
    gcloud container cluster delete <cluster-name>
