CLOUD IAM
Identity & Access Management

- you have resources in the cloud eg virtual server, database etc
- you have identities these can be human and non-human that need to access those resources and perform actions. eg launch, start, terminate virtual servers
- how do you identify users in the cloud. How do you configure the resources they can access?
- In gcp, Identity and Access Management IAM provide these services.
- IAM is all about Authentication (is this the right user) and Authorization (do they have the right access)
- Identities can be;
  A GCP User (Google Account or Externally Authenticated User).
  A Group of GCP users. Generally, groups are recommended since it makes managing identities easier.
  An Application running in GCP.
  An Application running in your data center.
  Unauthenticated users.
- IAM provides you with granular control.
  Limit single user; to perform single action, on specific cloud resource, from specific IP during specific time.

Cloud IAM Example

- I want to provide access to manage a specific cloud storage bucket to a colleague;
  Important Generic Concepts;

  - member: colleague
  - resource: cloud storage bucket
  - action: upload/delete objects
    The above concepts are general to every cloud.

  GCP IAM has these concepts;

  - roles: a set of permissions (to perform specific actions on specific resources). Roles do not know members. They do not have idea who they will be assigned to. It is all about permissions.
  - policy: this is how you assign permissions to a member. you assign/bind a role to a member.

- Steps:

1. Choose a Role with right permissions (ex: storage object admin)
2. Create a Policy binding the member with role permissions.

IAM Roles.
these are permissions to perform set of actions on some set of resources.
are of three types

1. Basic Roles (or Primitive Roles) - Owner/Editor/Viewer. Viewer(roles.viewer)-read only actions, Editor(roles.editor) viewer + edit actions i.e CRUD operations, Owner(roles.owner) - editor, manage roles and permissions and billings permissions. Full access to most GCP resources. Earliest versions before IAM was created. They are not recommended in production.
2. Predefined Roles - Fine grained roles predefined and managed by Google. There are different roles for different purposes. Examples, Storage Admin-grant full access of buckets and objects, Storage Object Admin-Full access to objects but not buckets, Storage Object Viewer-access to view objects, list objects in a bucket, Storage Object Creator-allows user to create objects, does not have permission to view, delete,or replace.
3. Custom Roles - When predefined roles are not sufficient, you can create your own custom roles. make sure to give custom roles rememberable ID since you will be using this ID to bind a role to a user in a policy.

IAM Predefined Roles - Example Permissions

1. Cloud Storage Roles
   Storage Admin-roles/storage.admin:storage.buckets*, storage.objects*
   Storage Object Admin-roles/storage.objectAdmin:storage.objects\*
   Storage Object Creator-roles/storage.objectCreator:storage.objects.create
   Storage Object Viewer-roles/storage.objectViewer:storage.objects.get
   All four roles have permissions to
   - resourcemanager.projects.get
   - resourcemanager.projects.list

IAM - Important Concepts - Review
Member: who?
Roles: Permissions (What actions? What resources?)
Policy: Assign permissions to members: map roles(what),to members(who?), and conditions(which resource?,when?,from where?)
Permissions are not directly assigned to members. Permissions are represented by a Role. Members get permissions through the role.

IAM Policy
Roles are assigned to users through IAM policy documents
Represented by a policy object which has list of bindings which binds a role to a list of members.
Member type is identified by prefix: eg user, serviceaccount, group, or admin.

Playing with IAM
Describe Current Project with gcloud

1. gcloud compute set projec <project name>
2. gcloud compute project-info describe <current project>
   will list the services account and all the portals sorrounding it.
3. gcoud auth login: will give access to gcp with google user credentials
4. gcloud auth revoke: revokes access credentials for an account.
5. gcloud auth list: list all the authorized accounts
6. gcloud projects;

- gcloud projects add-iam-policy-binding: Adds IAM policy binding. gcloud projects add-aim-policy-binding cp-k8s-398512 --member=user:bicosteve4@gmail.com --role=roles/storage.objectAdmin
- gcloud projects get-iam-policy <project-name>: Get IAM policy for a project.Returns member and service account role. All the bindings particular to that project. Will also have the users and their roles.
- gcloud projects remove iam-policy-binding p-k8s-398512 --member=user:bicosteve4@gmail.com --role=roles/storage.objectAdmin
- gcloud projects delete <project-id>: Removes a project

7. glcoud iam

- gcloud iam roles describe roles/storage.objectAdmin: Describes an IAM role
- glcoud iam roles create --project --permissions --stage: creates an iam role.
- gcloud iam roles copy --source=rles/storage.objectAdmin --destination=my.custom.role --dest-project=cp-k8s-398512: copies IAM Roles.

Service Accounts
Consider a scenario where an application on a VM needs access to cloud storage.
You should not use personal credentials to allow access for the VM.
The recommended approach is to use service account.
Service account will be identified by an email address eg id-compute@developer.gserviceaccount.com
When a compute engine or app engine is created, a service account is created for it too.
The service account will also have all the permissions associated with the compute engine or app engine and will have specific email address too.
It does have password associated with it
Uses public and private key.
You cannot use service account to login in with a browser or cookie. You will have to assign it to a VM and the VM will use it to make a call to a respective resources.
There are few types of service accounts;

1. Default Service Account;
   automatically created when some services are used.
   it is not recommended since it has Editor role.

2. User Managed;
   user can create his/her service account.
   this is recommended since you can assign fine grained roles.
   after creation, the service account can be assigned roles.
   you can then assign your service account to VM instances.

3. Google Managed Service Accounts;
   used by GCP to perform operations on users behalf.
   in general, we DONT need to worry about them.

Service Account Use Cases.

1. VM to Talk to Cloud Storage;
   Create service account role with right permissions
   Assign Service Account role to VM instance when it is starting up.
   Key generation and use are automatically handle by IAM when we assign a service account to the instance.
   No need for store credentials in the config files.
   **Do NOT delete** service accounts used by running instances.

2. On Premise to Cloud Storage (Long Lived)
   Connecting an on premise marchine to cloud storage.
   You **CANNOT ASSIGN SERVICE ACCOUNT** directly to an on premise application marchine
   Create a Service Account with right permissions
   Create service account with user managed keys on the console on 'Service Accounts' tab.
   Can also use gcloud iam service-accounts keys create
   Donwload the service account key file and keep it secure.
   You cannot regenerate the key again
   Make the servie account key file accessible to your application
   Set an environment variable GOOGLE_APPLICATION_CREDENTIALS
   set it to the path of the key file
   export GOOGLE_APPLICATION_CREDENTIALS='/path_to_key_file/'
   Use Google Cloud Client Libraries. The application default is Application Default Credentials(ADC)
   ADC uses the servie account key file if env var GOOGLE_APPLICATION_CREDENTIALS exists

3. On Premise to Google Cloud APIs (Short Lived)
   You want to make calls from outside GCP to Google Cloud APIs with short lived permissions.
   Permission for few hours or shorter.
   Has less risks compared to sharing service account keys.
   There few credential types recommended;
   **OAuth 2.0 access tokens**
   **OpenID Connect ID tokens**
   **Self-signed JSON Web Tokens (JWT)**
   Examples;
   (a) When a member needs elevated permissions, he can assume the service account role (Create OAuth 2.0 access toke for service account). Service account with short lived access will be created to assist with this. This gives temporary access only.
   (b) OpenID Connect ID tokens is recommended for service to service authentications. A Service in GCP needs to authenticate itself to a servic in other cloud.

Service Account Scenarios;

1. Scenario: Application on a VM wants to talk to a Cloud Storage Bucket
   Solution: Configure the VM to use a Service Account with the right permissions.
2. Scenario: Application on a VM wants to put a message on a Pub Sub Topic
   Solution: Configure the VM to use a Service Account with the right permissions
3. Scenario: Is Service Account and identity or a resource?
   Solution: It is both. You can attach roles with Service Account(identity). You can let other members access a SA by granting them a role on the Service Account(resource).
4. Scenario: VM instance with default Service Account in project A needs to access Cloud Storage bucket in project B.
   Solution: In project B, add the service account from project A and assign Storage Object Viewer Permissions on the bucket.

Access Control Lists (ACL)
ACL defines who has access to your buckets and objects as well as at what level of access they have.

How is it different from IAM?
IAM permissions apply to all objects within a bucket.
ACL can be used to customized specific accesses to different objects.
NB: User gets access if he is allowed by either IAM or ACL
NB: Use IAM for common permissions to all objects in a bucket.
NB: Use ACLs if you need to customize access to individual objects.

Access Control - Overview
How do you control access to objects in a Cloud Storage bucket?
Two types of access controls;

1. Uniform (Recommended) - Uniform bucket level access using IAM.
2. Fine-grained - Use IAM and ACLs to control access;
   both bucket level and individual object level permissions.

- Use uniform access when all users have same level of access across all objects in a bucket.
- Fine grained access with ACLs can be used when you need to customize the access at an object level.

Cloud Storage - Signed URL
You want to allow **limited time access** to a user to your objects.
Users do not need Google accounts.
Use **Signed URL** functionality.
A URL that gives **permissions to limited time duration** to perform specific actions.
To create a Signed URL;

1. Create a key (YOUR_KEY) for the Service Account/User with the desired permissions.
2. Create Signed URL with the key;
   gsutil signurl -d 10m YOUR_KEY gs://BUCKET_NAME/OBJECT_PATH

Cloud Storage - Static Websites
This will assist in exposing a static website to the web.
Steps;

1. Create a bucket with the **same name** as website name (Name of bucket should match DNS name of the website). Verify you own the dormain name.
2. Copy the files to the bucket. You can add index and error html files for a better user experience.
3. Add member allUsers and grant Storage Object Viewer option. Do this on permissions tab on the bucket and add allUsers. Select allow public access.
