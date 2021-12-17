# automatically-shutdown-and-restart-OpenShift-Container-Platform-cluster 

This repository provides an automated way to perform  shutdown and restart of an Openshift cluster on a cloud provider for cost saving reasons. This is being tested for OpenShift Container 4.x in the first version on AWS and can be extended to work with other cloud providers. There are two playbooks used to perform the shutdown and restart of the cluster. The shutdown is all done from with a cluster using a kubernetes cron job while the restart is done via an Ansible playbook ran as a cron job on a host to restart the instances and ensures that the cluster nodes and operators are up and running.   


Play Variables
--------------

For the cronjob playbook the following variables are used:
- cluster_shutdown_job_namespace: The name of the namespace or project the cronjob will be deployed in OpenShift to perform the cluster shutdown.
- cluster_shutdown_job_namespace_description: Decription of the namespace object. Default to the name of the namespace.
- cluster_shutdown_service: The name of the service and the application used to run the cronjob. 
- cluster_shutdown_service_account: The name of the service account used to run the cronjob. 
- ocp_cluster_user: The username of the cluster-admin user to use to run this playbook.
- ocp_cluster_user_password: The password associated with the username above.
- ocp_cluster_console_url: The URL to the API for the cluster this will be deployed to.
- ocp_cluster_console_port: The API port for the cluster this will be deployed to. Default to 6443 when not set.
- openshift_cli: The path to the OpenShift client binary used to run the commands use to intereact with the cluster. It could be the path to kubectl or oc. Default to oc when not set assuming that the client on on your path.
- aws_vpc_id: The ID of the AWS VPC the cluster resources are created and running in .
- aws_rhcos_ami: The RHCOS AMI ID of the RHCOS image used to provision and run the instances to be shutdown. This is used a filter to find the cluster instances.
- cluster_shutdown_template_path: The path to the OCP template used to deploy the cron job used to shutdown the cluster.
- cluster_shutdown_image_registry_repository: The Registry and repository name where the image used to run thr cron job is hosted. 
- cluster_shutdown_image_name: The name of the image used to run the cron job. This uses ose-tools-rhel8 by default.
- cluster_shutdown_cron_expression: The cron expression used to set the schedule used to run the job that shutdown the cluster.

There are few other optional variables that you can change to your liking but don't have to change. 
For the crontab playbook used to restart the instances the following variables are used:
- aws_rhcos_ami: The RHCOS AMI ID of the RHCOS image used to provision and run the instances to be shutdown. This is used a filter to find the cluster instances.
- cluster_shutdown_template_path: The path to the OCP template used to deploy the cron job used to shutdown the cluster.
- setup_cron_job: Used the determine whether to setup the cron tab or not. Default to no and the instances are restarted now.
- cron_job_runtime_day: The day on which to run the crontab job. It has to be a valid cron expression day.
- cron_job_runtime_hour: The time the cron tab is to be run. It has to be a valid cron expression hour.
- cron_job_runtime_minute: The minute the cron tab is to be run. It has to be a valid cron expression minute.

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.


Installation and Usage
-----------------------
Clone the repository to where you want to run this from and make sure you can connect to your cluster using the CLI . 
You will also need to have cluster-admin run in order to run the playbook since the cron job need to run privileged.
Finally before running the playbook make sure to set and update the variables as appropriate to your use case.

Playbooks
---------
To run the main playbook that creates a kubernetes cronjob to shutdown the cluster, use the ansible-playbook command as follows   
`ansible-playbook deploy-cluster-shutdown-cronjob.yml --ask-vault-pass -vvv`  
The above playbook creates a kubernetes cronjob that shutdown the cluster based on the cron expression provided.
To run the playbook that creates a cronjob to restart the cluster, use the ansible-playbook command as follows   
`ansible-playbook restart-cluster.yml --ask-vault-pass -vvv`  
The above playbook creates a crontab process under /etc/cron.d that restart the instances on the defined cron expression using the AWS cli command.



License
-------

BSD

Author Information
------------------

