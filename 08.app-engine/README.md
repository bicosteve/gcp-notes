# APP ENGINE

- the simplest way to deploy and scale applications on GCP.
- provides end-to-end application management.
- provides pre-configured runtimes for most types of programming languages.
- can use custom run time and write code in any language.
- connects to a variety of GC storage products like Cloud Sql etc.
- no usage charges however you pay for resources provisioned.

**Features;**

1. Automatic load balancing and auto-scaling.
2. Managed platform updates & application health monitoring.
3. Provide application versioning.
4. allows traffic splitting between the versions of the applications running.

**Compute Engine vs. App Engine**
Compute Engine

- Infrastructure as a service
- More flexible
- More responsibility to you e.g. choosing images, installing software, choosing hardware, fine grain access, permissions, availability, etc.

App Engine

- Platform as a service.
- Serverless
- Lesser Responsibility
- Lower Flexibility

## App Engine Environments

- 1. Standard: Applications run in language-specific sandboxes. Has complete isolation from OS/Disk/Other Apps.
     Has a V1 version which supports the older versions of the programming languages. Has restricted network access and only whitelisted extensions and libraries are allowed.
     There are no restrictions for Java and Go runtimes.
     Has a V2 version which supports newer versions of the programming languages. Provide full network access and no restrictions on language extensions.

- 2. Flexible: Application instances run within Docker Containers.
     Make use of Compute Engine virtual machines.
     Supports ANY runtime eg Python, Go, Java etc.
     Provides access to background processes and local disks.

## App Engine - Application Component Hierarchy

- Application -> Service -> Version -> Instance

1. Application - can only have one app per project. It is the container for everything you create.
2. Services - Multiple microservices or app components. Can have multiple services in a single application. Each service can have a different setting. Earlier, services were called modules.
3. Version - version is associated with code and configurations. Each version can run in one or more instances. Multiple versions can exist at the same time. There is an option to split traffic and rollback.

## App Engine Comparison

1. Pricing factors: Instance hours for standard and vCPU, memory & persisten disk for flexible.
2. Scaling: Manual, basic, automatic for standard and manual,automatic for flexible.
3. Scaling to Zero: with standard you can scale to zero instances while for flexible no minimum one instance.
4. Instance Startup time: it is seconds for standard and minutes for flexible.
5. Rapid scaling: for standard you can rapidly scale but for flexible you cannot.
6. Maximum request time out: 1-10 mins for standard and 60 minutes for flexible.
7. Local Disk: Most can write to /tmp for standard and ephemeral. New disk on startup for flexible.
8. SSH for debugging: Standard does not allow while flexible allows.

## App Engine Scaling Instances

There are different ways of scaling app engine instances.

1; Automatic Scaling.

- this is automatically scaling instances bases on the load.
- it is recommended for continously running workloads.
- can auto scale based on;
  - Target CPU Utilization - configure a CPU usage threshhold.
  - Target Throughput Utilization - configure a throughput threshold.
  - Maximum Concurrent Request - configure maximum concurrent requests an instance can receive.
- configure Max Instances & Min Instances.

2; Basic

- Creates instances when requests are received.
- It is recommended for adhoc workload.
- When there is no load/request instances are shut down.
- High latency is possible because new instances have to be created when requests are made.
- It is not supported by app engine flexible.
- can configure max instances to go upto and idle time out.

3; Manual

- configure specific number of instances to run.
- adjust number of instances manually over time.

## App Engine Demo

- create a project in gcp
- enable app engine api
- create an app engine project
- open cloud shell editor
- drag and drop your project or create your project from the editor.
- check for the projects configs with gcloud config list
- set your project with gcloud config set project PROJECT_ID
- cd into the folder which has project entry point
- run gcloud app deploy <.yaml> file
- select 'y'
- the app will be deployed.
- run gcloud app browse
- check running services with gcloud app services list
- check runnign version with gcloud app versions list
- check for app instances with gcloud app instances list
- deploy a new version with gcloud app deploy --version=v2
- check app url with gcloud app browse --version="version"

## App Engine Split Traffic between versions

- deploying to a new version and switching traffic to it is a bad idea.
- you need to deploy new version and slowly switch traffic to it.
- gcloud app deploy --version=v3 --no-promote
- NB: default is promote which transfers traffic immediately
- to check new version use; 'gcloud app browse --version v3' to get the url
- split traffic between the two versions
- gloud app services set-traffic splits=v3=.5,v2=.5
- traffic is split by ip address
- you can split traffic by a different option
- gloud app services set-traffic splits=v3=.5,v2=.5 --split-by=random
- to set traffic to v3, gcloud app services set-traffic --splits=v3
- to see the requests use watch curl <app_url>

## Configure Service Name For Deploy

- you can configure a service name in app.yaml file then use this to deploy the app
- use gcloud app deploy while inside the folder with the app.yml file
- You can stream logs from the command line by running:
- gcloud app logs tail -s bix-service
- to view the application in the browser run
- gcloud app browse -s <service-name>
- gcloud app browse --service="<service-name>"

## Things to Configure in app.yaml file

runtime: python28 - the name of the runtime environment used by your app
api_version: 1 - recommended - specify on gcloud app deploy -v [version]
instance_class: F1
service: service-name
env: flex - this for containers

inboud_services:

- warm-up

env_variables:
ENV_VARIABLE: 'value'

handlers:

- url: /
- script: home.app

automatic_scaling:
target_cpu_utilization: 0.65
min_instances: 5
max_instances: 100
max_concurrent_requests: 50

basic_scaling:
max_instances: 11
idle_timeout: 10m
manual_scaling:
instances: 5
