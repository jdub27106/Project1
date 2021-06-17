## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/19pdMcEcCCd0rn0Z5Ald1Wun5yd5t6MXn/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the installbeats.sh file may be used to install only certain pieces of it, such as Filebeat.

#!bin/bash

#Install and configure filebeat
ansible-playbook filebeat/filebeat-playbook.yml

#Install and configure metricbeat
ansible-playbook metricbeat/metricbeat-playbook.yml

#Install and configure auditbeat
ansible-playbook auditbeat/auditbeat-playbook.yml

#Install and configure packetbeat
ansible-playbook packetbeat/packetbeat-playbook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting inbound access to the network.
Load balancers protect against DDoS atacks by shifting away the attacking traffic. The advantage of having a jumpbox is it gives access to the admin from a single system. This allows the admin to secure and monitor the network constantly.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.

Filebeat watches for log files and locations while collecting log events. Metricbeat records metric and statistical data from the operating system (OS) and from other services running on the server.

| Name       | Function   | IP Address | Operating System |
|------------|------------|------------|------------------|
| Jump Box   | Gateway    | 10.0.0.5   | Linux            |
| Web 1      | Web Server | 10.0.0.7   | Linux            |
| Web 2      | Web Server | 10.0.0.8   | Linux            |
| Web 3      | Web Server | 10.0.0.9   | Linux            |
| ELK-Server | Elk Server | 10.1.0.5   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

	75.183.33.111

Machines within the network can only be accessed by each other.

	Web 1, Web 2, and Web 3 all send traffic to the ELK-Server

A summary of the access policies in place can be found in the table below.

| Name       | Publicly Accessible | Allowed IP Addresses     |
|------------|---------------------|--------------------------|
| Jump Box   | Yes                 | 75.183.33.111            |
| Web 1      | No                  | 10.0.0.5                 |
| Web 2      | No                  | 10.0.0.5                 |
| Web 3      | No                  | 10.0.0.5                 |
| ELK-Server | Yes                 | 10.0.0.* & 75.183.33.111 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because playbooks allow us to configure multiple virtual machines (VMs) through a single command script. Also because everything can be done from a container in the Jump Box.

The playbook implements the following tasks:

	-Create a new VM
	-Download/configure the ELK-docker container to matche the 	IP address you will use for the ELK-Server
	-Create a new ansible-playbook (elk.yml) that will set-up 	the ELK-Server VM to map to ports 5601 9200 5044. You can 	check using the command 'sudo docker ps'
	-Create new inbound security rules in your Security group(s) 	to allow 5601 and 9200 on your personal network
	-Connect to ELK using 'http://[ELK-Server 	IP:5601]/app/kibana]'

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.


(Images/Dockerps.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

	-10.0.0.7
	-10.0.0.8
	-10.0.0.9

We have installed the following Beats on these machines:
	
	-Filebeat
	-Metricbeat
	-Auditbeat
	-Packetbeat

These Beats allow us to collect the following information from each machine:

	-Detects change to filesystem. Also collects Apache logs
	-Detects change in filesystem metrics like CPU usage and SSH 	ogin attempts
	-Audits the activities of users and processes from the Linux 	udit Framework. Detecting changes to critical files
	-Collects packets that pass through the NIC tracing all activity on the network

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook files to appropriate directories
- Update the config files for each beat to include ELK-Server host info
- Run the playbook, and navigate to http://[ELK-ServerIP]:5601/app/kibana

Common Questions
- _Which file is the playbook? Where do you copy it?_ filebeat-playbook.yml copied to /etc/ansible/filebeat-config.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? /etc/filebeat/filebeat-config.yml
 How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ edit filebeat-config.yml file
- _Which URL do you navigate to in order to check that the ELK server is running? http://[ELK-ServerIP]:5601/app/kibana

