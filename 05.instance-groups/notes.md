# Instance Groups

- These are groups of VMs managed as a single entity.
- you can manage group of similar vms having similar lifecycle as ONE UNIT.
  There are two type of Instances Groups

1. Managed Instance Group
   They are identical VMs created using a template.
   They have the same machine type, same image and same configurations.
   Features provide are Auto Scaling, auto healing and managed releases.

2. Unmanaged Instance Group
   You can have different configurations for VMs in the same group.
   Does not offer auto scaling or auto healing & other services.
   They are not recommended unless you need different kinds of VMs

- Location of Instance Groups can be Zonal or Regional

## Managed Instance Groups

- identical vms created using an instance template.
- Important features:
- maintain certain number of instances eg two vms running, if one fails it will auto heal.
- detects application failure using health check. it can auto heal.
- can increase or decrease instances based on load - auto scaling.
- can add Load Balancer to distribute load.
- can create instances in multiple zones (regional MIGs).
- regional MIGs provide higher availability compared to zonal MIGs.
- can release new application versions without downtime.
- has rolling updates meaning release new version step by step
- can update a percentage of instances to the new version at a time.
- canary deployment - test new version with a group of instances before releasing it across all instances.
-

### Creating Managed Instance Groups

- instance template is mandatory.
- configure auto scaling to automatically adjust number of instances based on load:
  - must have minimum number of instances.
  - maximum number of instances.
  - auto scaling metrics i.e CPU utilization target or LoadBalancer utilization target or any other metric from Stack Driver.
  - cool-down period; how long to wait before looking at auto scaling metrics again.
  - scale in controls; prevent a sudden drop in no of VM instances eg don't scale by more than 10% or 3 instances in 5 mins.
  - can configure auto healing; can configure a health check with initial delay and how long to wait for your app to to initialize before running a health check. This will check unhealthy instances and replace them immediately.
  - Initital delay is done for a VMs to wait before it accepts traffic.
  - Managed instance group are of two types;
    a) Stateless - automatically manage groups of VMs that do stateless serving and batch processing
    b) Stateful - Automatically manage groups of VMs that have persistent data or configurations eg database or legacy applications.

### Updating Managed Instance Group

- 1. Rolling Update - this gradual update of instances in an instance group to the new instance template. You need to specify a new template or optionally specify a template for canary testing. Specify How Updates Should Be Done - start the update immediately (Proactive) or when an instance group is resized later. (Opportunistic). How should the update happend. Maximum surge; how many instances are added at any point in time. Can also set the number of instances that can be offline during the update-maximum unavailable.

- 2. Rolling Restart/Replace - Gradual restart or replace of all instances in the group. There is no change in template but you want to replace/restart the existing VMs.

### Instance Group Scenarios

- 1. **Scenario:** You want MIG managed application to survive zonal failures.
- **Solution:** Create multiple zone MIG (or regional MIG)
- 2. **Scenario:** You want to create VMs of different configurations in the same group.
- **Solution:** Create un-managed instance groups.
- 3. **Scenario:** You want to preserve VM state in an MIG
- **Solution:** Stateful MIG-Preserver VM state(instance name, attached persistent disk, metadata). Recommended for stateful workloads like database, data processing apps
- 3. **Scenario:** You want a high availability in an MIG even when there are hardware/software updates.
- **Solution:** Use an instance tempale with availability policy automatic restart enabled & oh-host maintenance; migrate ensure live migration and automatic restarts.
- 4. **Scenario:** You want unhealthy instances to be automatically replaced
- **Solution:** Configure a health on the MIG (self healing)
- 5. **Scenario:** Avoid frequent scale ups and scale downs
- **Solution:** Cool down period and initial delay

### Managed Instance On The Terminal

- 1. to check for managed instance groups
- gcloud compute instance-groups managed <option>
- eg gcloud compute instance-groups managed list
- 2. create managed instance group
- gcloud compute instance-groups managed create my-mig --zone us-central1-a --template my-instance-template --size 1.
- eg gcloup compute instance-groups managed create my-mig --zone us-central1-a --template rest-api-template --size 2
- you can add the --health-check flag with --health-check=HEALTH_CHECK
- you can add --initial-delay for how much time should be given before the instance start
- other commands are gcloud compute instance-groups managed delete/describe/list
- you can set up the auto scaling with;
- gcloud compute instance-groups managed set-autoscaling my-mig --max-num-replicas=10
- gcloud compute instance-groups managed set-autoscaling my-mig --min-num-replicas=4
- you can stop auto scaling with;
- gcloud compute instance-groups managed stop-autoscaling my-mig --zone us-central1-a
- update existing MIG policies;
- gcloud compute instance-groups managed update my-mig
