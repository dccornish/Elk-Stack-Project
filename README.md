# Elk-Stack-Project
Ansible, Docker, Elk Stack
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Azure_Lab_Environment](https://user-images.githubusercontent.com/80214918/110392801-23f4dc00-802f-11eb-80b8-1d03ceef61ce.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat and Microbeat.

 - root@4d2c8d5a2466:~# /etc/ansible/roles/filebeat-playbook.yml
 - root@4d2c8d5a2466:~# /etc/ansible/roles/elk-playbook.yml
 - root@4d2c8d5a2466:~# /etc/ansible/roles/metricbeat-        playbook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting traffic to the network.

- What aspect of security do load balancers protect? 
  
Load balancers protect against DDoS attacks by shifting attack traffic to a separate cloud network.

- What is the advantage of a jump box?

Allows admins quickly access other points of the network with less restrictions. They can also be in a different security zone.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- What does Filebeat watch for? Filebeat is a lightweight shipper for forwarding and centralizing log data.

- What does Metricbeat record? Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.


| Nam
Name
Function
IP
Operating System
Jump Box
Gateway
10.0.0.1
Linux
Web 1
Server
10.0.0.10
Linux
Web 2
Server
10.0.0.9
Linux
Web 3
Server
10.0.0.2
Linux
Elk
Server
10.1.0.4
Linux

      

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- Add whitelisted IP addresses: 98.164.88.98

Machines within the network can only be accessed by Jump Box.

- Which machine did you allow to access your ELK VM? Jump Box

- What was its IP address? 98.164.88.98

A summary of the access policies in place can be found in the table below.


NAME
PUBLICLY ACCESSIBLE
ALLOWED IP ADDRESS
Jump Box
Yes
10.0.0.1
Web 1
No
10.0.0.10
Web 2
No
10.0.0.9
Web 3
No
10.0.0.2
Elk
No
10.1.0.4

                   

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- What is the main advantage of automating configuration with Ansible? It is seamless and easy to apply configurations to new and multiple virtual machines. 

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation
- ... Create ELK VM in Azure
- ... SSH to Jump Box
- ... Attach Ansible 
- ... SSH to ELK VM and run playbooks

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.



### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.10
- 10.0.0.9
- 10.0.0.2

We have installed the following Beats on these machines:
- Filebeat was successfully installed
- Metricbeat was successfully installed

These Beats allow us to collect the following information from each machine:

- Filebeat is used to collect log files from specific files on remote machines. An example would be MS Azure and MySQL databases. 

- Metricbeat collects machine metrics. It is a measurrement to analyse how healthy the system is

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

---Filebeat---

- Copy the filebeat-configuration.yml file to /etc/ansible/roles/files.

- Update the filebeat-configuration.yml file to include the ELK private IP in lines 1106 and 1806.

- Run the playbook, and navigate to http://20.36.137.243:5601/app/kabana(ELK-VM public IP) to check that the installation worked as expected.

---Metricbeat---

- Copy the metricbeat-configuration.yml file to /etc/ansible/roles/files.

- Update the metricbeat-configuration.yml file to include the ELK private IP in lines 62 and 96.

- Run the playbook, and navigate to http://20.36.137.243:5601/app/kabana(ELK-VM public IP) to check that the installation worked as expected.


Answer the following questions to fill in the blanks:
- Which file is the playbook? filebeat-playbook.yml

- Where do you copy it? /etc/ansible/roles

- Which file do you update to make Ansible run the playbook on a specific machine? etc/ansible/hosts

- How do I specify which machine to install the ELK server on versus which to install Filebeat on? in the etc/ansible/hosts file two groups must be specified. One group is are the webservers which have the IPs of the VMs, and the other group is elkservers.

- Which URL do you navigate to in order to check that the ELK server is running? http://20.36.137.243:5601/app/kabana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

  -------Filebeat---------

- To create the filebeat-configuration.yml file: nano filebeat-configuration.yml. For this, I used the filebeat configuration file template.

- To create the playbook: nano filebeat-playbook.yml

  ---
 - name: installing and launching filebeat
	   hosts: webservers
       become: true
       tasks:

	   - name: download filebeat deb
  	     command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.7.1-amd64.deb

	   - name: install filebeat deb
  	     command: dpkg -i filebeat-7.7.1-amd64.deb

	   - name: drop in filebeat.yml
  	     copy:
   	       src: ./files/filebeat-configuration.yml
   	       dest: /etc/filebeat/filebeat.yml

	   - name: enable and configure system module
  	     command: filebeat modules enable system

	   - name: setup filebeat
  	     command: filebeat setup

	   - name: start filebeat service
  	    command: service filebeat start
---
- To run the playbook: ansible-playbook filebeat-playbook.yml

* In order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/filebeat-playbook.yml


-------Metricbeat-------

- To create the metricbeat-configuration.yml file: nano metricbeat-configuration.yml. For this, I used the metricbeat configuration file template.

- To create the playbook: nano metricbeat-playbook.yml

---
  - name: installing and lunching metricbeat
    hosts: webservers
    become: true
    tasks:
    
  - name: download metricbeat deb
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.7.1-amd64.deb
    
  - name: install metricbeat deb
    command: sudo dpkg -i metricbeat-7.7.1-amd64.deb
    
  - name: drop in metricbeat.yml
    copy:
      src: /etc/ansible/roles/files/metricbeat-configuration.yml
      dest: /etc/metricbeat/metricbeat.yml
      
   - name: enable and configure system module
     command: metricbeat modules enable system
     
   - name: setup metricbeat
     command: metricbeat setup
     
   - name: start metricbeat service
     command: service metricbeat start
     
   ---
   
   - To run the playbook: ansible-playbook metricbeat-playbook.yml
   
   * In order to run the playbook, you have to be in the directory where the playbook is, or give the path to it (ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml

