# 2nd Generation Cloud Functions

2 product versions
cloud functions 1st gen.
cloud functions 2nd gen.

- recommended use is the 2nd gen.

## Key Enhancement in 2nd Gen.

- longer request time out; upto 60 minutes.
- larger instace sizes; upto 16gb of ram with 4 cpus
- concurrency; upto 1000 concurrent request per function instance.
- multiple function revisions and trafffic splitting.
- support 90+ event types.
- this will connect to cloud run

## Scaling and Concurrency

- auto scaling is possible in serverless architecture. New invocations come in, new function instances are created.
- one function instances handle ONLY ONE request at a time.
- cold start; a new function instance from scratch can take some time.
- takes longer to start a function that is why it is called cold start.
- configure minimum number of instances but this will increase cost.
- 2nd gen instances can handle multiple requests at the same time.
- can configure how many invocations a function can handle with max of 1000

## Gcloud Cloud Functions Deployment

- gcloud functions deploy [NAME]
- supports a wide variety of functions like --docker-registry
- supports docker repository --docker-repository
- supports generations like --gen2
- supports runtime like --runtime
- supports service account like --service-account
- supports max and min instances
- supports source code --source like zip file or source repo
- supports trigger like --trigger-bucket
