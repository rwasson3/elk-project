## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/rwasson3/elk-project/blob/main/elkproject/images/network%20diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above, or select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

https://github.com/rwasson3/elk-project/blob/main/elkproject/filebeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

What aspect of security do load balancers protect? What is the advantage of a jump box?_

Load balancing plays an important role in cloud security, maintaining a stable site, and defending against DDoS attacks. It does this by identifying an attack and shifting traffic away from the corporate server to a public cloud server.

A jumpbox provides a secure and reliable platform to which system admins can first connect before running admin tasks. It can also be used as a [sort of] rally point to connect to other servers outside.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

Filebeat monitors log files or specified locations, collects log events, and forwards them for cataloging.

Metricbeat collects and compiles metrics and statistics and puts them into a format specified by the admin. Metricbeat collects metrics from the system and services running on a server to monitor in order to monitor that server.

The configuration details of each machine may be found below.

| name       	|   	| function                             		|   	| ip address 	|   	| os    	|
|------------	|---	|--------------------------------------		|---	|------------	|---	|-------	|
| jumpbox    	|   	| gateway and runs docker with ansible 	|   	| 10.0.0.4   	|   	| linux 	|
| elk server 	|   	| runs elk containers                  		|   	| 10.1.0.4   	|   	| linux 	|
| web1       	|   	| web server to run DVWA               	|   	| 10.0.0.5   	|   	| linux 	|
| web2       	|   	| web server to run DVWA               	|   	| 10.0.0.6   	|   	| linux 	|


### Access Policies

The machines on the internal network are not exposed to the public internet. 

Only the jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP address: 68.186.15.98

Machines within the network can only be accessed by going through the jumpbox.

Which machine did you allow to access your ELK VM? What was its IP address? jumpboxprovisioner: 20.239.131.189

A summary of the access policies in place can be found in the table below.

| name       	|   	| publicly accessible? 	|   	| allowed ip addresses 	|
|------------	|---	|----------------------		|---	|----------------------		|
| jumpbox    	|   	| yes                  		|   	| 68.186.15.98      		|
| elk server 	|   	| yes [via port 5601] 	 	|   	| 20.239.131.189    		|
| web1       	|   	| no                   		|   	| 10.0.0.4             		|
| web2       	|   	| no                   		|   	| 10.0.0.4             		|

### Elk Configuration

Ansible helps with the representation of infrastructure as code [iac]. It helps to automate the process so the machines have the same configuration. Ansible was used to entirely automate configuration of the ELK machine. None of the configuration was performed manually, which is advantageous because it prevented the inevitable mistakes inherent with manually inputting data, and made for a more efficient process. Automation also gave more control over what was installed and tasks performed on the machine.

The playbook implements the following tasks:
The first task is to install the docker.io on Elk
Python is installed next,also on Elk
Since Elk requires more memory, the playbook is used to increase memory
The next task is to tell the Elk virtual to use the increased memory
Downloads, installs, and executes the docker Elk container on the Elk virtual upon restart, automating the Elk startup. This enables docker on startup, eliminating the need to manually start docker each time you startup your virtual

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/docker_ps_image.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines: web1: 10.0.0.5 & web2 10.0.0.6

We have installed the following Beats on these machines: Filebeat & Metricbeat

These Beats allow us to collect the following information from each machine:

Filebeats collects slow logs, audit logs, gc logs,deprecation logs, and server logs. Metricbeats collects data from the services running on the server and the system. It then puts them into the output you specify.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to the ansible container: /etc/ansible/roles/filebeat.playbook.yml
- Update the filebeat-config.yml file to include host "10.1.0.4:9200" with username "elastic" and password "changeme" and setup.Kibana host to "10.1.0.4:5601"
- Run the playbook, and navigate to http://13.75.215.90:5601/app/kibana to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it? Filebeat_playbook.yml and you copy it to /etc/ansible/roles/filebeat.playbook.yml

- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on? The file you update to make Ansible run the playbook is Filebeat-config.yml. To specify which machine to on which toinstall the ELK server, you edit the hosts file to distinguish between the different servers such as “elk” and “web servers”
 
- _Which URL do you navigate to in order to check that the ELK server is running? http://20.83.33.24:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.:

To create/update the playbook: nano playbookname.yml
To run the playbook: ansible-playbook playbookname.yml


