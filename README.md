# Elk-Stack-Project
Ansible, Docker, Elk Stack
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![Azure_Lab_Environment](https://user-images.githubusercontent.com/80214918/110512134-8f3fbc00-80ca-11eb-917b-6a555fa3a71c.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat and Microbeat.

 - root@4d2c8d5a2466:~# /etc/ansible/roles/filebeat-playbook.yml
 - root@4d2c8d5a2466:~# /etc/ansible/roles/elk-playbook.yml
 - root@4d2c8d5a2466:~# /etc/ansible/roles/metricbeat-playbook.yml

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

![13 table 1](https://user-images.githubusercontent.com/80214918/110512938-505e3600-80cb-11eb-9f3a-d9c5f3632b0e.png)

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- Add whitelisted IP addresses: 98.164.88.98

Machines within the network can only be accessed by Jump Box.

- Which machine did you allow to access your ELK VM? Jump Box

- What was its IP address? 98.164.88.98

A summary of the access policies in place can be found in the table below.

![13 table 2](https://user-images.githubusercontent.com/80214918/110513126-7edc1100-80cb-11eb-8143-60b0372f6763.png)

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually.

- What is the main advantage of automating configuration with Ansible? It is seamless and easy to apply configurations to new and multiple virtual machines. 

The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation
- ... Create ELK VM in Azure
- ... SSH to Jump Box
- ... Attach Ansible 
- ... SSH to ELK VM and run playbooks

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![13 docker ps](https://user-images.githubusercontent.com/80214918/110513200-95826800-80cb-11eb-9567-5966051e52a8.PNG)

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

- Metricbeat collects machine metrics. It is a measurement to analyze how healthy the system is

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

## ---Filebeat---

- Copy the filebeat-configuration.yml file to /etc/ansible/roles/files.

- Update the filebeat-configuration.yml file to include the ELK private IP in lines 1106 and 1806.

- Run the playbook, and navigate to http://20.36.137.243:5601/app/kibana(ELK-VM public IP) to check that the installation worked as expected.

## ---Metricbeat---

- Copy the metricbeat-configuration.yml file to /etc/ansible/roles/files.

- Update the metricbeat-configuration.yml file to include the ELK private IP in lines 62 and 96.

- Run the playbook, and navigate to http://20.36.137.243:5601/app/kibana(ELK-VM public IP) to check that the installation worked as expected.
Answer the following questions to fill in the blanks:
- Which file is the playbook? filebeat-playbook.yml

- Where do you copy it? /etc/ansible/roles

- Which file do you update to make Ansible run the playbook on a specific machine? etc/ansible/hosts

- How do I specify which machine to install the ELK server on versus which to install Filebeat on? in the etc/ansible/hosts file two groups must be specified. One group is are the webservers which have the IPs of the VMs, and the other group is elkservers.

- Which URL do you navigate to in order to check that the ELK server is running? http://20.36.137.243:5601/app/kabana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.

## -------Filebeat---------

- To create the filebeat-configuration.yml file: nano filebeat-configuration.yml. For this, I used the filebeat configuration file template.

- To create the playbook: nano filebeat-playbook.yml

![13 filebeat](https://user-images.githubusercontent.com/80214918/110515842-746f4680-80ce-11eb-8255-491dc912f38d.png)
        
---
- To run the playbook: ansible-playbook filebeat-playbook.yml

* In order to run the playbook, you have to be in the directory the playbook is at, or give the path to it (ansible-playbook /etc/ansible/roles/filebeat-playbook.yml

## -------Metricbeat-------

- To create the metricbeat-configuration.yml file: nano metricbeat-configuration.yml. For this, I used the metricbeat configuration file template.

- To create the playbook: nano metricbeat-playbook.yml

![13 metricbeat](https://user-images.githubusercontent.com/80214918/110515058-95836780-80cd-11eb-8dac-d2eec9de7600.png)

   ---
   
   - To run the playbook: ansible-playbook metricbeat-playbook.yml
   
   * In order to run the playbook, you have to be in the directory where the playbook is, or give the path to it (ansible-playbook /etc/ansible/roles/metricbeat-playbook.yml)

