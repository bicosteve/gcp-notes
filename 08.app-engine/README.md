# APP ENGINE

- simplest way to deploy and scale applications on GCP.
- provides end to end application management.
- provides pre configured runtimes to for most types of programming languages.
- can use custom run time and write code in any language.
- connects to variety of GC storage products like Cloud Sql etc.
- no usage charges however you pay for resources provisioned.

**Features;**

1. Automatic load balancing and auto scaling.
2. managed platform updates & application health monitoring.
3. provides application versioning.
4. allows traffic splitting between the versions of the applications running.

**Compute Engine vs App Engine**
Compute Engine

- Infrastructure as a service
- More flexible
- More resposibility to you eg choosing image, installing software, choosing hardware, fine grain access, permissions, availability etc.

App Engine

- Platform as a service.
- Serverless
- Lesser Responsibility
- Lower Flexibility

## App Engine Environments

- 1. Standard: Applications run in a language specific sandboxes. Has complete isolation from OS/Disk/Other Apps.
     Has V1 version which support the older versions of the programming languages. Has restricted network access and only whitelisted extensions and libraries are allowed.
     There are no restrictions for java and go runtimes.
     Has V2 version which support newer versions of the programming languages. Provide full network access and no restrictions on language extensions.

- 2. Flexible: Application instances run within Docker Containers.
     Make use of Compute Engine virtual machines.
     Supports ANY runtime eg Python, Go, Java etc.
     Provides access to background process and local disks.

## App Engine - Application Component Hierarchy.

- Application -> Service -> Version -> Instance

1. Application - can only have one app per project. It is the container for everything you create.
2. Services - Multiple microservices or app components. Can have multiple services in a single application. Each service can have a different setting. Earlier, services were called modules.
3. Version - version is associated with code and configurations. Each version can run in one or more instances. Multiple versions can exist at the same time. There is option to split traffic and rollback.

## App Engine Comparison
