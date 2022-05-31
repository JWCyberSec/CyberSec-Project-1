# CyberSec-Project-1
Collection of Ansible YAML scripts, Bash scripts, Cloud Security and Networking network diagrams
## Automated ELK Stack Deployment

The files in this repository were used to configure the networks depicted below.

![Cyber_Security / Network Diagrams](CyberSec-Project-1/Diagrams)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - CyberSec-Project-1/Ansible

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting access to the network. What aspect of security do load balancers protect? What is the advantage of a jump box?
-Load Balancers protect against the potential of DDoS attacks (Denial of Service). The job of a load balancer is to analyze incoming traffic and determine where the traffic is sent, without overloading one server in particular. Distributing traffic evenly maintains efficiency of the network, and from the perspective of a client request, improves application responsiveness. A jump box allows greater control over who has potential access to a VM (virtual network). It acts as a secure computer that all admins first connect to before launching any administrative task or use as an origination point to connect to other servers or untrusted environments. It allows System Administrators the ability to test what might occur when the Cloud Network is active and running without the potential risk from hacking or bad actors upon the system. 
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.
-  What does Filebeat watch for?
Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing
- What does Metricbeat record?
Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify such as Elasticsearch or Logstash. Metricbeat helps you monitor your servers by collecting metrics from the system and services running on the server such as Apache.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| JumpBox | Gateway & runs Docker/ Ansible | 137.116.137.99| Linux  |
| Web 1    | Web Server used to run DVWA  |  10.0.0.5 | Linux  |
| Web 2    | Web Server used to run DVWA  |   10.0.0.6| Linux  |
| ELKVM    | Run Elkstack Container and Kibana|20.243.70.100| Linux | 

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox VM machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 98.204.186.176

Machines within the network can only be accessed by Jumpbox VM.
- Which machine did you allow to access your ELK VM? What was its IP address?
 Jumpbox VM has the only access to the ELK VM (137.116.137.99)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes/No              | 98.204.186.176    |
| Web1     | No                  | 137.116.137.99    |
| Web2     | No                  | 137.116.137.99    |
| ELKVM    | Yes                 | 20.243.70.100

### Elk Configuration

- Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it is very simple to setup, execute, and use. It requires no special coding skills.

The playbook implements the following tasks:
- The ELK playbook installs docker.io on the ELK virtual machine
- Python is installed on the ELK VM (also run by the ELK playbook)
- ELK playbook specifies an increase in virtual memory to run the machine more smoothly
- Downloads, installs and executes the docker elk container on the ELK VM on restart so the elk container doesn't need to be manually started
-This enables docker on boot so you don't have to manually start docker when you turn VM back on

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

!Images/docker_ps_output.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.5 (Web1)
10.0.0.6 (Web2)

We have installed the following Beats on these machines:
Filebeat
Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat collects log information about the file system and specifies which files have been changed and when a file was changed (to Logstash or Elasticsearch). To check for the output of Filebeat, the ELK log manager Kibana checks logs for changes over a period of time (set by the user). 
- Metricbeat shows statistical information for the processes running on your system including memory and CPU usage. Using Kibana you can more easily decipher the data collected and review the metrics of the system
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat file to etc/ansible/files/filebeat-config.yml
- Update the filebeat-config.yml file to include host changes (10.1.0.4:9200)(username 'elastic' password 'changeme') (setupKibana host 10.1.0.4:5601)
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?
/etc/ansible/roles/filebeat-playbook.yml  & /etc/ansible/roles/metricbeat-playbook.yml
These files would be copied to /etc/filebeat/filebeat-playbook.yml & /etc/metricbeat/metricbeat-playbook.yml respectively

- _Which file do you update to make Ansible run the playbook on a specific machine?
/etc/ansible/files/filebeat-config.yml & /etc/ansible/files/metricbeat-config.yml

 How do I specify which machine to install the ELK server on versus which to install Filebeat on? 
The ELK will be running on a separate machine as it is receiving logs from all three VM's (Jumpbox, Web1 Web2). This is the importance of having the ELKVM (Elk Stack Virtual Machine) on a separate Vnet (Virtual Network). This can be configured in the Jumpbox by changing the /etc/ansible/hosts file by adding the private IP address for the ELKVM to a specified [elk] host (under the [webservers] host in # EX 2). Filebeat can be configured for whatever host you want to gain its logging information from by making changes to the /etc/ansible/files/filebeat-config.yml


  

- _Which URL do you navigate to in order to check that the ELK server is running?
http://20.243.70.100:5601/app/kibana (http://ELKVM Public IP/app/kibana)

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
