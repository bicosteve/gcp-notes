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

- check for configurations with gcloud config configurations list
- will return all the configurations associated with your account
- gcloud config configurations create <configuration name>
- gcloud config configurations create my-configuration
- this will create configurations and activate it
- set project with configurations
- gcloud config set project <my_project>
- will set the project in the current configuration
- activate a configuration with gcloud config configurations activate my-default-configurations
