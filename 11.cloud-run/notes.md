# WHAT IS CLOUD RUN

- assist in deploy containers to production in seconds!
- it is useful while deploying a stateless container
- service name and region once allocated cannot be deleted
- CPU allocation and pricing
  can allocate cpu only during requests
  can allocate cpu always and this will keep on incurring costs
- build on top of an open standard - Knative
- fully managed serverless platform for containerized applications.
- has zero infrastructure management
- it is pay per use
- fully integrated end to end developer experience.
- no limit in languages, birnaries and dependancies
- easily portable becase of container based architecture
- cloud code, cloud building, cloud monitoring, and cloud logging integrations.
- supports yml configurations as kubernetes.

# ANTHOS

- the idea behind anthos is to run k8s clusters anywhere.
- cloud, multi cloud, private on premise
- cloud run for anthos enable you to deploy workload on anthos cluster running on-premises or on Google cloud

# Cloud Run on Command Line

Deploy a new container;
gcloud run deploy SERVICE_NAME --image IMAGE_URL --revision-suffix v1
first deployment creates a service and first revision
next deployments for the same service creates new revisions

List Avaialbe Revisions;
gcloud run versions list

Assigning Traffic Assignments;
gcloud run services update-traffic myservice --to-revisions=v2=10,v1=90
