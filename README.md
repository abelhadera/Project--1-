# Stack automation with ELK deployment
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/abelhadera/Project--1-/blob/49c1429e773455fb9dbf6d8bbe7e73fc1f7b8642/Red-team-diagram.jpg


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Project-1 file may be used to install only certain pieces of it, such as Filebeat.

https://github.com/abelhadera/Project--1-/blob/75e64dfc9e93eb01fe969a738d1ffcc75e2d8560/Ansible-playbook.png

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly functional, in addition to restricting huge amount of traffic to the network.
 What aspect of security do load balancers protect? What is the advantage of a jump box?_


It help prevent the servers from being overcrowded while optimizes the availability of the Web servers in case of the event of one website being shutdown the other will be up and running. With the Jump-Box being the centralized location for configuration and holding docker/ansible container for "easy" maintence

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the system.
- What does Filebeat watch for?_ * Filebeat monitors log files that I specify to watch and sends the data to the "Elasticsearch"
-  What does Metricbeat record?_  * Metricbeat records statistics and also sends the data to the "Elasticsearch"

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | Server   | 10.0.0.6   | Linux            |
| Web-2    | Server   | 10.0.0.7   | Linux            |
| ELK      | Server   | 10.0.1.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump-Box-Provisionermachine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 73.106.176.190 (My local IP) 

Machines within the network can only be accessed by the Jump-Box-Provisioner.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_
*Jump-Box-Provisioner, public ip was 13.92.40.49 and private 10.0.0.5
A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 |      73.106.176.190  |
| Web-1    | No                  |       10.0.0.5       |
| Web -2   | No                  |       10.0.0.5
| ELK      | No                  |       10.0.0.5       |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because
*One advantage is that it is able to configure multiple machines at once i.e both my web servers with different configurations made to a yaml playbook.
*Another advantage is that it is an easier alternative to manage the machines when dealing with multiple different setups and configurations

The playbook implements the following tasks:
- First step is to SSH into the Jum-Box-Provisioner VM to access the ansible container; ssh sysadmin@13.92.40.49

-  Next is to start ansible; sudo docker start loving_cannon

-Then to attach it(logging in); sudo docker attatch loving_cannon

-Next go to /etc/ansible directory and create the install-elk playbook

-Run the newly made playbook while in the same directory; ansible-playbook install-elk.yml

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/abelhadera/Project--1-/blob/9f7498cb64f8664b1695acfe86b47e1ac7c6d226/Docker-ps.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring_
  
  *10.0.0.6 ( web1) 10.0.0.7 (web)

We have installed the following Beats on these machines:
 Specify which Beats you successfully installed_

*Filebeat

These Beats allow us to collect the following information from each machine:


* Filebeat is used to collect lof files from the specific path you chose on remote machines.

* An example of this would be files that are generated by Windows Server, or Azure tools and then logged back into elastic for research.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the filebeat-config.yml file to _/etc/ansible.

- Update the_filebeat-config.yml file to include the private IP in lines 1106 and 1806

- Run the playbook, and navigate to 13.64.129.125 to check that the installation worked as expected.

- _Which file is the playbook? Where do you copy it?
-       
-         _filebeat-playbook.yml; /etc/ansible
- 
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

- /etc/ansible/hosts then put the two IP of the Virtual machines (10.0.0.6,10.0.0.7);

In the etc/ansible/host file one group will be name [webservers] that has your two webservers you want (10.0.0.6,10.0.0.7) while the other group [elkservers] which will have the ip of the VM i want elk installed to (10.1.0.4)
- _Which URL do you navigate to in order to check that the ELK server is running? 
- *
- The public ip of the ELK server 13.64.129.125



  -------Filebeat---------

-create the filebeat-configuration.yml file: nano filebeat-configuration.yml; and make the apporiate configurations in the file provided 

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
-To run the playbook: ansible-playbook filebeat-playbook.yml



