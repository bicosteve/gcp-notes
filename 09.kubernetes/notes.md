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

13. Important Notes

    1. Cluster - this where you run your workload. Must create a cluster
       it is a group of compute engine instances.
       has master node and workers
       worker nodes run workload or pods.
       Master node has;

    a). Api Server - which handles all the communications for k8s cluster from nodes and outside
    b). Schedular - decides placement of pods.
    c). Control Manager - manages deployments and replicasets
    d). etcd - distributed database storing the state of the cluster
    Worker nodes;
    this is where pods run
    kubelet - worker nodes talk to master node through kubelets

    Types of Cluster
    Zonal Cluster;

    - single zone with single control plane. Nodes run in the same zone.
    - multi zone single control plane but nodes are running in multiple zones.

    Regional Cluster;
    Replicas of the control plane run in multiple zones of a given region.
    Nodes also run in same zones where control plane runs

    Private Cluster;
    Specific to virtual private cluster.
    Nodes only have internal IP addresses

    Alpha Cluster;
    Clusters with alpha APIs - early feature APIs. Used to test new K8S features

    2. Pods - are smallest deployable units in a K8s
       contains one or more containers.
       Each pod has one or more ephemoral IP address.
       most pods however only has one container
       all containers in the same pod share;

       - network
       - storage
       - ip address
       - ports
       - volumes
         Pods status can be running, pending, suceeded, failed, unknown

    3. Deployment
       a deployment is created for each microservice
       kubectl create deployment my-deployment --image=m1:v1
       single deployment represent a microservice with all its releases
       deployment manages new releases ensuring zero downtime
       update deployment image with;
       kubectl set image deployment k8s-rest-api k8s-rest-api=in28min/hello-world-rest-api:0.0.2.RELEASE

    4. ReplicaSet
       ensures that a specific number of pods are running for a specific microservice version.
       when you delete one pod in a cluster, the replicaset maintains the state number of replicasets
       even if one pod is killed, ReplicaSet will make sure the state of replicas is maintained as per configs

    5. Service
       each pod has its own IP address.
       when a pod fails, it is replaced by a replicaset.
       when things change internally in a cluster, it does not affect external users.
       services exposes your deployment to your users on the outside world.
       create a service with kubectl expose deployment <name> --type=LoadBalancer --port=9090
       services ensures external world is not affected when things are changing in a cluster
       there are various types of services
       a) ClusterIP - Exposes service on a cluster internal IP. This cannot be reached from outside world but internally in a cluster. This is for intra cluster communication.
       b) LoadBalancer - exposes a service externally using a cloud provider's load balancer.
       This will work in any cloud
       Use Case is when you want to create individual LoadBalancer for a microservice.
       c) NodePort - Exposes Service on each Node's IP at a static port (NordPort)
       Use case;
       you do not want to create an external LoadBalancer for each microservice
       you can create one Ingress component to load balance multiple microservices.

14. Container Registry/Image Repository
    you have created docker images for your services, where do you store them?
    they are stored in **_Container Registry_** provided by GCP.
    alternative is docker hub
    can be integrated to CI/CD tools and publish to container registry
    you can secure your container images, analyze vulnerabilities and enforce deployment policies
    naming convention: hostname/projectid/image:tag
    eg gcr.io/projectname/helloworld:1
    NB: container registry is private by default.

15. GKE to Remember
    always replicate master nodes across multiple zones for high availability.
    some CPU on the node is reseved by control plane
    creating a docker image for your microservices in Dockerfile
    you can build docker image with its version release
    you can run your docker image with docker run -d -p 8080:9090 in28min/hello-world-rest-api:0.0.1.RELEASE
    you can push it to container repository
    kubernetes supports stateful deployment like Kafka, Redis, ZooKeeper
    StatefulSet - set of pods with unique, persistent identities and stable hostnames.
    you can run services on nodes for log collection or monitoring
    use DaemonSet - One pod on every node. For background services
    cloud monitoring and cloud logging can be used to monitor your kubernetes cluster.

16. Scenarios
    a) You want to keep cost low and optimize GKE implementation
    consider preemtible VMs, Appropriate Region, Committed-use discounts
    E2 machine types are cheaper than N1
    choose right environment to fit your workload type
    b) You want efficient, completely auto scaling GKE solution
    configure horizontal pod autoscaler for deployments and cluster autoscaler for node pools.
    c) you want to execute untrusted third-party code ink8s clutster
    create a node pool and run it there so that it does not affect your cluster pool
    d) you want to enable ONLY internal communication between your microservice deployments in K8s cluster
    use ClusterIP as object service type
    e) my pod staying pending
    most probably Pod cannot be scheduled onto a node(insufficient resource)
    increase the number of nodes
    f) my pods staying waiting
    probably not able to pull the image.
    you can also have access issues to the container registry

17. Cluster Management On Command Line

    1. Creating a cluster;
       gcloud container clusters create <cluster-name> --zone <zone> --node-locations <location>
       gcloud container clusters create my-cluster --zone us-central1-a --node-locations us-central1-c,us-central1-b
    2. Resize the cluster;
       gcloud container clusters resize my-cluster --node-pool my-node-pool --num-nodes 10
       gcloud container clusters resize bix8s-cluster --node-pool qa-pool --num-nodes 5

    3. Autoscale Cluster;
       gcloud container clusters update cluster-name --enable-autoscaling --min-nodes=1 --max-nodes=5
       gcloud container clusters update bix8s-cluster --enable-autoscaling --min-nodes=2 --max-nodes=5
    4. Delete Cluster;
       gcloud container clusters delete bix8s-cluster
    5. Adding Node Pool;
       gcloud container node-pools create new-node-pool-name --cluster my cluster
    6. List Images;
       gcloud container images list

18. Workload Management
    **List Pods/Service/ReplicaSets;**
    kubectl get pods/services/replicasets -o wide
    **Create Deployment;**
    kubectl apply -f deployment-file or kubectl create deployment
    **Create Service;**
    kubectl expose deployment <deployment-name> --type=LoadBalancer --port=9090
    **Scale Deployment;**
    kubectl scale deployment <my-deployment> --replica 5
    **Auto Scale Deployment;**
    kubectl autoscale deployment --max=10 --min=2 --cpu-percent=80
    **Delete Deployment;**
    kubectl delete deployment <deployment-name>
    **Update Deployment;**
    kubectl apply -f deployment.yml
    **RollBack Deployment;**
    kubectl rollout undo deployment <deployment-name> --to-revision=1
    **Getting Projects**
    gcloud project list
    **Setting Project**
    gcloud config set project <project-id>

19. Delete GKE Resources
    **Delete Services**
    kubectl delete servive k8s-rest-api
    **Delete Deployment**
    kubectl delete deployment k8s-rest-api
    **Delete Clusters**
    gcloud container clusters delete <cluster-name> --zone <zone-name>
    gcloud container clusters delete gcp-k8s-cluster --zone southamerica-west1-b

20. Kubernetes Terminology - Review
    **_Hardware (Cluster)_**
    Master Node - manages the cluster
    Worker Node - run your workloads
    Node Pool - group of nodes in a cluster with the same configurations
    **_Software_**
    Pods - smallest deployable unit in kubernetes. Are deployed to Worker Nodes.
    Deployments - Manages the pods
    Service - exposes deployments
