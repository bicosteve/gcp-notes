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

## App Engine - Application Component Hierarchy.

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
