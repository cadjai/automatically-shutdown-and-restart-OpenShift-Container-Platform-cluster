# automatically-shutdown-and-restart-OpenShift-Container-Platform-cluster 

This repository provides an automated way to perform  shutdown and restart of an Openshift cluster on a cloud provider for cost saving reasons. This is being tested for OpenShift Container 4.x in the first version on AWS and can be extended to work with other cloud providers. There are two playbooks used to perform the shutdown and restart of the cluster. The shutdown and restart are done via an Ansible playbook ran as a cron job on the controller host to restart or shutdown the instances.   


Play Variables
--------------

For the crontab playbook used to restart the instances the following variables are used:
- aws_vpc_id: The ID of the AWS VPC the cluster resources are created and running in .
- aws_rhcos_ami: The RHCOS AMI ID of the RHCOS image used to provision and run the instances to be shutdown. This is used a filter to find the cluster instances.
- setup_cron_job: Used the determine whether to setup the cron tab or not. Default to no and the instances are restarted now.
- cron_job_runtime_day: The day on which to run the crontab job. It has to be a valid cron expression day.
- cron_job_runtime_hour: The time the cron tab is to be run. It has to be a valid cron expression hour.
- cron_job_runtime_minute: The minute the cron tab is to be run. It has to be a valid cron expression minute.

For the crontab playbook used to shutdown the instances the following variables are used:
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
To run the main playbook used to shutdown the cluster, use the ansible-playbook command as follows   
`ansible-playbook shutdown-cluster.yml --ask-vault-pass -vvv`  
The above playbook creates crontab process under /etc/cron.d that shutdown the cluster based on the cron expression provided.
To run the playbook that creates a cronjob to restart the cluster, use the ansible-playbook command as follows   
`ansible-playbook restart-cluster.yml --ask-vault-pass -vvv`  
The above playbook creates a crontab process under /etc/cron.d that restart the instances on the defined cron expression using the AWS cli command.



License
-------

BSD

Author Information
------------------

