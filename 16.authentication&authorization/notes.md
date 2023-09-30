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
-
