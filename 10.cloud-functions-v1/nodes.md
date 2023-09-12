# CLOUD FUNCTIONS

Google Cloud Functions is a serverless execution environment for building and connecting cloud services. With Cloud Functions you write simple, single-purpose functions that are attached to events emitted from your cloud infrastructure and services

### 1. Intro

functions run code in response to an event.
the event can be file upload, https invocation, messaging logging etc
business logic functions can be written in multiple languages like nodejs, go, java, ruby, .net etc
when using cloud functions, you do not need to worry about scaling or avaialability of the code.
when using cloud functions, you pay for only what you use like number of invocations, compute time of the invocations, memory and cpu
cloud functions are timebound and the default is 1 min and max of 60 mins configuration.
there are 2 product versions of cloud functions
1st gen cloud functions
2nd gen cloud functions which are build on top of cloud run and eventarc

### 2. Concepts

**Event** things like upload object to cloud storage, https request, queue message to pub/sub
**Trigger** respond to events with a function call. which function trigger when event happens. functions take event data dna perform action.
events are triggered from;
cloud storage
cloud pub/sub
http post/get/delete/put/options
NB: Cloud trigger url - https://us-central1-devops63827.cloudfunctions.net/bix-1st-function

### 3. Cloud Functions NB

- no server management - you do not need to worry about the scaling or availability of your function.
- cloud functions automatically spin up and back down in response to events.
- they scale horizontally
- are recommended when responding to events
- they are not idea for a long running processes
