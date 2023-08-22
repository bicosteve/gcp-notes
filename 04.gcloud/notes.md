## Gloud

#### 1. What is Gcloud

- this is command line tool to interact with google cloud resources
- most gcp services can be managed from the CLI using Gcloud
- these are Compute Engine VMs, Managed Instance Groups, Databases, etc
- you can do CRUD on existing resources and perform actions like deployment as well.
- some gcp services have specif CLI tools.
  Cloud Storage - gsutil
  Cloud BigQuery - bq
  Cloud BigTable - cbt
  Kubernetes - kubectl in addition to Gcloud for managing clusters
- it is part of Google Cloud SDK
  it requires python to run
- you can also gcloud from 'cloud shell'
- also has online Code Editor
- use gcloud init to initialize gcloud in your terminal.
- this has allowed gcloud to use my credentials
- gcloud config list it will show your current default region for compute
- also shows default zone, account details and project

#### 2. Config Set

- gcloud config list will show your account, project name and environment.
- gcloud config list account: will show the account being used.
- to check region, use gcloud config list compute/region. Will return region if set and region (unset) if not set.
- core is the config section for any gcloud account
- gcloud config core/account; will return the account details.
- gcloud config set; it is used to set specified property in your active configuration
- gcloud config set core/project VALUE
- can be used to set region, zone, project
- gcloud config set core/project PROJECT
- gcloud config set compute/region REGION
- gcloud config set compute/zone ZONE
- gcloud config set core/verbosity VALUE(debug)
- NB; while using gcloud always run gcloud config list first before you do anything to know which project you are in.
- get help by using gcloud config list --help
- the opposite to set is unset
- gcloud config unset <>
- go get list of regions run gcloud compute regions list
- to get list of zones run gcloud compute zones list
- to set a compute zone use gcloud config set compute/zone me-central1-a

#### 3. Multiple Configurations

Scenario: You are working on multiple projects from the same machine. You would want to be able to execute commands using different configurations. Easily switch from projects

- check for configurations with 'gcloud config configurations list'
- will return all the configurations associated with your account
- gcloud config configurations create <configuration name>
- gcloud config configurations create my-configuration
- this will create configurations and activate it
- set project with configurations
- gcloud config set project <my_project>
- will set the project in the current configuration
- activate a configuration with gcloud config configurations activate my-default-configurations

#### 4. Gcloud Command Structure

- takes this format gcloud GROUP SUBGROUP ACTION ...
- GROUP - config or compute or container or dataflow or functions or iam or . These are services.
- SUBGROUP - instances or images or instance-templates or machine-types or regions or zones.
- ACTION - create or list or start or stop or describe or . what you want to do.
- example - gcloud compute instances list: list all the compute instances.
- example - gcloud compute instances create first-instance.
- will create first instance with specific region on config on gcloud.
- example - gcloud compute instances describe first-instance: give details around the specific instance.
- example - gcloud compute instances delete first-instance.
- example - gcloud compute zones list
- example - gcloud compute regions list
- example - gcloud compute machine-types list
- example - gcloud compute machine-types list --filter='zone:us-central1-b'
- example - gcloud cmpute machine-types list
- filter multiple zones gcloud compute machine-types list --filter zone:"(asia-southeast2-b asia-southeast2-c)"

#### 5. gcloud compute instances create: in-depth

- create and instance -> gcloud compute instances create my-instance
- options:
  --machine-type: gcloud compute instances create my-instance --machine-type e2-highcpu-4
  --custom-cpu --custom-memory --custom-vm-type(n1/n2) (custom machine)
  gcloud compute instances crate my-instance --custom-cpu 6 --custom-memory 3072MB --custom-vm-type n2
  --image or --image-family or --source-snapshot or --source-instance-template or --source-machine-image(beta)
  --service-account --no-service-account
  --tags
  --zone=us-central1-b
  --preemptible
  --restart-on-failure(default) --no-restart-on-failure --maintainance-policy(Migrate(default)/Terminate)
  --boot-disk-size --boot-disk-type --boot-disk-auto-delete --no-boot-disk-auto-delete
  --deletion-protection --no-deletion-protection(default)
  --metadata/metadata-from-file startup-script/startup-script-url
  ------shutdown script
  --network --subnet --network-tier
  --accelerator="type=nvidia-tesla-v100,count=8"

#### 6. Compute instances - default region and zone

- There are three options
  1. Option 1 - Centralized Configuration: gcloud compute project-info add-metadata. This can also be set
     in the gcp console on VM -> Settings ->
     --metadata=[google-compute-default-region=REGION | google-compute-default-zone=ZONE]
  2. Option 2 - Local Configuration: gcloud config set compute/region REGION
  3. Option 3 - Command Specific: --zone or --region in the command
     NB: option 3 if exists ovveride option 2 if exists overrides option 1

#### 7. List & Describe Commands

- list commands are used to 'list' a set of resources.
- gcloud compute 'resources' list
- gcloud compute instances list, gcloud compute images list
- list command suppport few common options
- eg gcloud compute zone list --filter=region:us-west2
- gcloud compute zones list --sort-by=region
- describe commands: gives more information about a specific resources

#### 8. Playing with Compute Instances

- playing with compute instances includes;
- gcloud compute instances list/start/stop/delete/reset/describe/move
- gcloud compute instances stop example-instance
- gcloud compute instances delete example-instance. Attach a --delete-disks=VALUE(all or data or boot)
- gcloud compute instances move example-instance-1 --zone us-central1-b --destination-zone us-central1-c

#### 9. Instance Template

- gcloud compute instance-templates create/delete/describe/list
- gcloud compute instance-templates list
- gcloud compute instance-templates create instance-example: will be created with default configuration
- gcloud compute instance-templates delete instance-example
- gcloud compute instance-templates describe instance-example
