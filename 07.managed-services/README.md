# What Are Managed Services?

Managed services are services that offer partial or complete management of a client's cloud resources or infrastructure. Management responsibilities can include migration, configuration, optimization, security, and maintenance.
These contains;

## 1. Infrastructure As A Service (IAAS)

- when you are using infrastructure as a service, you are only using infrastructure from cloud provider.
- example using VMs to deploy your applications or database.
- IAAS is responsible for;
  - application code and runtime.
  - configuring load balancing.
  - auto scaling
  - Os upgrade and patches
  - Availability etc

## 2. Platform As A Service

- when you are using platform as a service, the cloud provider is responsible for;
  - OS upgrades and patches
  - application runtime
  - auto scaling, availability and load balancing.
- you are only resposible for;
  - configuration of application and services
  - application code.
- examples of this are AWS Elastic bean stalk, Google App Engine.
- here you only focus on the application code.
- eg if you are deploying java application, you just provide the code for the specific service and cloud provider will ensure the right OS is configured for your application.
- they are of different varieties;
  1. CAAS - Container as a Service; containers instead of apps.
  2. FAAS - Function as a Service; functions instead of apps.
  3. Databases - Relational and Non Relational.
- Opt for PAAS when you are working in the cloud.

  ### 1. Containers As A Service

  - to understand CAAS, you will need to understand microservices.
  - instead of large monolith applications, enterprises build small focused micro services eg movieservice,review service, booking service, fare calculation service.
  - this is flexible and innovative since application can be build in different programming languages.
  - however, the deployment becomes complex.
  - CONTAINERS assist in deploying applications with different services.
  - among containers are Docker containers.
  - with docker, you can create images with the images container all the needs of a microservice.
  - the image containes application runtime, application code dependancies.
  - with the image, you can run it on any infrastructure eg localhost, data center, or cloud.
  - containers are light weight as they do not have guest OS.
  - containers are cloud neutral.

  #### Container Orchestration

  - Requirements: I want 10 instances of microservice A container, 15 instances of microservice B container
  - You will need container orchestration for these instances.
  - the orchestrator can be Kubernetes among others.
  - it will be responsible for deployment of these containers into clusters.
  - the orchestrator offers numbers of features like;
    - auto scaling -> scale containers based on demands.
    - service discovery -> helps microservices find one another.
    - load balancer -> distribute load among multiple instances of a microservice.
    - self healing -> does health check and replace the failing machines.
    - zero downtime deployments -> release new versions without downtime.

## 3. Serverless

- When an application is developed, we think about where to deploy, kind of servers, what OS, auto scaling and availability of the application.
- what if we do not need to worry about the servers and focus only on the code?
- this is where serverless enters.
- does not mean no servers just that you do not have visibility of the server.
  **Serverless for me:** - You do not worry about infrastructure (Zero visibility into the infrastructure). Flexible scaling and automated high availability for free - Pay for use with no request for service zero cost.

  - with serverless, you pay for requests and not the servers.
  - focus on the code and the cloud managed services takes care of all that is needed to scale your code to serve requests.
  - example AWS lambda, Azure function and Google Function

  **Serverless Perspective**

  - Important features
  - 1. Zero worry about infrastructure, scaling and availability.
  - 2. Zero requests that is zero cost when requests are not made.
  - 3. Pay for requests and not instances.

## 4. GCP Managed Services for Compute

| Month | Details |Category |
|**\*\*\*\***\_\_\_**\*\*\*\***|**\*\***\*\***\*\***\*\***\*\***\*\***\*\***\_**\*\***\*\***\*\***\*\***\*\***\*\***\*\***|**\*\***\_\_**\*\***|
| Compute Engine | High Performance general purpose VMs that scale globally | IaaS |
| Kubernetes Engine | Orchestrator for containerized microservices. | CaaS |
| App Engine | Build scalable applications on managed platform | PaaS |
| Cloud Functions | Build event driven applications with single purpose function| FaaS |
| Cloud Run | Develop & Deploy containerized applications. Dont need cluster| CaaS |
|
