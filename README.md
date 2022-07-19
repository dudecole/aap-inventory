Red Hat Ansible Automation Platform Deployment
==============================================

## Source
<TODO: interesting how the formatting works
on this..--dcole

Yes, I have taken this from the my brain and 
actually made something of it..  This comment 
needs to be removed as this is just a test.
>

## Inventory Repository
This is the main AAP inventory and can be used
for other inventories.  This should be auto-imported
into AAP, and groups/hosts created in the AAP 
Inventories/Hosts.

<NOTE: Keeping inventories in their own separate project 
can make things a little easier to keep track of. >

## Purpose
Goal is to have Ansible deploy/re-deploy the AAP cluster 
during a GitOps process of some sort.  Currently this is
built for Gitlab's webhook notification that is sent 
to the AAP Controller Job Template Webhook URL.  

<NOTE: 
- find documentation/URL for creating a WH URL in a AAP Job Template
- integrating w/ and without Gitlab/Github AAP integrations
>


## setup.sh

This script and some of the contents of this repository come from
the original installation files in the AAP installation
bundle/pack.
<TODO: 
- adding the controller collection playbooks to the Job Template
  - test/fix?
>

## setup.yml
Was created as a wrapper to the setup.sh to place into a runnable
AAP resource, like a Job Template.  

Reason for this was to run a Job Template triggered via GLab
webhook notification.  When the notification is received, the 
`setup.yml` file is started.  

<TODO: 
- having issues with stdout/callback coming from the setup.sh or farther
nested playbook like - from the collection.ansib..playbook.install.yml  
>

<NOTE: 
- get the FQCN of collection controller installer...blah
- look into using the collection playbook
  - may require reverse eng`ing the `setup.sh` AAP 2.x 
    installer script.
>


<NOTE:
- add the `cop HA playbooks` for unregistering/removing AAP Cluster
nodes
- 
>

This collection of files provides a complete set of playbooks for deploying
Red Hat Ansible Automation Platform in your environment on hosts running
Red Hat Enterprise Linux.

For getting started with installation and setup instructions, please refer to:

- Install Guide -- https://access.redhat.com/documentation/en-us/red_hat_ansible_automation_platform/2.2/html-single/red_hat_ansible_automation_platform_installation_guide/index
>
