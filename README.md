# Project--1-
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - _TODO: Enter the playbook file._

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
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?_
*It help prevent the servers from being overcrowded while optimizes the availability of the Web servers in case of the event of one website being shutdown the other will be up and running

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
- _TODO: What does Filebeat watch for?_ * Filebeat monitors log files that I specify to watch and sends the data to the "Elasticsearch"
- _TODO: What does Metricbeat record?_  * Metricbeat records statistics and also sends the data to the "Elasticsearch"

The configuration details of each machine may be found below.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.1   | Linux            |
| Web-1    | Server   | 10.0.0.6   | Linux            |
| Web-2    | Server   | 10.0.0.7   | Linux            |
| ELK      | Server   | 10.0.1.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the __Jump-Box-Provisioner __machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 73.106.176.190 (My local IP) 

Machines within the network can only be accessed by the Jump-Box-Provisioner_____.
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

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
*One advantage is that it is able to configure multiple machines at once i.e both my web servers with different configurations made to a yaml playbook.
*Another advantage is that it is an easier alternative to manage the machines when dealing with multiple different setups and configurations

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- First step is to SSH into the Jum-Box-Provisioner VM to access the ansible container; ssh sysadmin@13.92.40.49
- Next is to start ansible; sudo docker start loving_cannon
-Then to attach it(logging in); sudo docker attatch loving_cannon
-Then go to /etc/ansible directory and create the install-elk playbook
-Run the newly made playbook while in the same directory; ansible-playbook install-elk.yml

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_
  *10.0.0.6 ( web1) 10.0.0.7 (web)

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_
*Filebeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
* Filebeat is used to collect lof files from the specific path you chose on remote machines.
* An example of this would be files that are generated by Windows Server, or Azure tools and then logged back into elastic for research.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _filebeat-config.yml____ file to _/etc/ansible____.
- Update the _filebeat-config.yml____ file to include the private IP in lines 1106 and 1806
- Run the playbook, and navigate to __13.64.129.125__ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_filebeat-playbook.yml; /etc/ansible
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_/etc/ansible/hosts then put the two IP of the Virtual machines (10.0.0.6,10.0.0.7);
*In the etc/ansible/host file one group will be name [webservers] that has your two webservers you want (10.0.0.6,10.0.0.7) while the other group [elkservers] which will have the ip of the VM i want elk installed to (10.1.0.4)
- _Which URL do you navigate to in order to check that the ELK server is running? *The public ip of the ELK server 13.64.129.125

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

*nano filebeat-playbook.yml
---
- name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:
    # Use command module
  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

    # Use command module
  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

    # Use copy module
  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

    # Use command module
  - name: Enable and Configure System Module
    command: filebeat modules enable system

    # Use command module
  - name: Setup filebeat service
  - command: service filebeat start 

    # Use systemd module
  - name: Enable service filebeat on boot 
  - systemd:
        name: filebeat
        enabled: yes

*run ansible-playbook filebeat-playbook.yml (in the same directory)


