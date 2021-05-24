# Project-ELK-ARV
\## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Diagrams/ELKNetworkDiagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the file may be used to install only certain pieces of it, such as Filebeat.

https://github.com/atithivirmani/Project-ELK-ARV/tree/main/Ansible

This document contains the following details:

- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build


\### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the Damn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

- Load balancers protect systems from DDoS attacks like SYN flood by shifting traffic and improving the resiliency of systems.

The advantage of a jump box is to give access to user from a single node that can be secured and monitored.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat takes the metrics and statistics that it collects and ships them to the output that is specified, such as Elasticsearch or Logstash

The configuration details of each machine may be found below.

Note: Use the [Markdown Table Generator] to add/remove values from the table.

| Name      | Function     | IP Address      | Operating System |

|-----------|--------------|-----------------|------------------|

| Jump Box  | Gateway      | 10.0.0.1        | Linux            |

| Web1      |Web server    | 10.0.0.8        | Linux            |

| Web2      |Web server    | 10.0.0.9        | Linux            |

| ELK       |ELK Server    | 10.2.0.4        | Linux            |

| LB        |Load Balancer | 191.239.178.214 | Linux            |

|User System|Access Control| 14.201.15.150   | Windows          |

| Web3      |              | 10.0.0.13       | Linux            |


\### Access Policies

The machines on the internal network are not exposed to the public Internet.

Only the Elk Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- 14.201.15.150 over Port 5601

Machines within the network can only be accessed by Workstation and Jump-Box-Provisioner.

- Which machine did you allow to access your ELK VM? What was its IP address?

Jump-Box-Provisioner IP : 10.0.0.7 via SSH port 22

Workstation Public IP :  14.201.15.150 via port TCP 5601

A summary of the access policies in place can be found in the table below.

Name 	            Publicly Accessible 	       Allowed IP Addresses

Jump Box 	              No 	                      14.201.15.150 on SSH 22

Web1 	                  No 	                      10.0.0.8 on SSH 22

Web2                    No                        10.0.0.9 on SSH 22

Web3 	                  No 	                      10.0.0.13 on SSH 22

ELK Server 	            No 	                      14.201.15.150 using TCP 5601

Load balancer 	        No 	                      14.201.15.150 on HTTP 80


\### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- Ansible lets you quickly and easily deploy multi-tier apps. You won't need to write custom code to automate your systems; you list the tasks required to be done by writing a playbook, and Ansible will figure out how to get your systems to the state you want them to be in.

The playbook implements the following tasks:

Specify a different group of machines as well as a different remote user

- name: Config elk VM with Docker

hosts: elk

remote\_user: sysadmin

become: true

tasks:

Increase System Memory :

- name: Use more memory

sysctl:

name: vm.max\_map\_count

value: '262144'

state: present

reload: yes

Install the following services:

`docker.io`

`python3-pip`

`docker`, which is the Docker Python pip module.

Launching and Exposing the container with these published ports:

`5601:5601`

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Diagrams/ELK%20running.PNG

The status should be up.


\### Target Machines & Beats

This ELK server is configured to monitor the following machines:

- Web1: 10.0.0.8

Web2: 10.0.0.9

Web3: 10.0.0.13

We have installed the following Beats on these machines:

- ELK Server, Web1, Web2 and Web3

The ELK Stack Installed are: FileBeat and MetricBeat

These Beats allow us to collect the following information from each machine:

Filebeat: log events
https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Diagrams/Filebeat.PNG

Metricbeat: metrics and system statistics
https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Diagrams/Metricbeat.PNG


\### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

For ELK VM Configuration:

- Copy the Ansible ELK Installation and VM Configuration
- Run the playbook using this command : ansible-playbook install-elk.yml

For FILEBEAT:

Download Filebeat playbook usng this command:

curl -L -O https://gist.githubusercontent.com/slape/5cc350109583af6cbe577bbcc0710c93/raw/eca603b72586fbe148c11f9c87bf96a63cb25760/Filebeat > /etc/ansible/files/filebeat-config.yml

Copy the '/etc/ansible/files/filebeat-config.yml' file to '/etc/filebeat/filebeat-playbook.yml'

Update the filebeat-playbook.yml file to include installer

curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-amd64.deb

Run the playbook using this command ansible-playbook filebeat-playbook.yml and navigate to Kibana > Logs : Add log data > System logs > 5:Module Status > Check data to check that the installation worked as expected.

For METRICBEAT:

Download Metricbeat playbook using this command:

curl -L -O https://gist.githubusercontent.com/slape/58541585cc1886d2e26cd8be557ce04c/raw/0ce2c7e744c54513616966affb5e9d96f5e12f73/metricbeat > /etc/ansible/files/metricbeat-config.yml

Copy the /etc/ansible/files/metricbeat file to /etc/metricbeat/metricbeat-playbook.yml

Update the filebeat-playbook.yml file to include installer

curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.6.1-amd64.deb

Update the metricbeat file rename to metricbeat-config.yml

Run the playbook, (ansible-playbook metricbeat-playbook.yml) and navigate to Kibana > Add Metric Data > Docker Metrics > Module Status to check that the installation worked as expected.

Answer the following questions to fill in the blanks:\_

- Which file is the playbook? Where do you copy it?

For the ANSIBLE : We will create the my-playbook1.yml as our playbook.

For FILEBEAT: We will create filbeat-playbook.yml as our playbook.

For METRICBEAT: We will create metricbeat-playbook.yml as our playbook.


- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

You need to update the hosts file within /etc/ansible to run the playbook on a specific machine

You create a [elk] group in /etc/ansible/hosts for VM on which to run ELK. You specify the VM IP in FileBeat config file on which to install Filebeat.

- Which URL do you navigate to in order to check that the ELK server is running?

Test Kibana on web : http://[your.ELK-VM.External.IP]:5601/app/kibana

Test Kibana on localhost: sysadmin@10.2.0.4: curl localhost:5601/app/kibana


\_As a \*\*Bonus\*\*, provide the specific commands the user will need to run to download the playbook, update the files, etc.

COMMAND 	                                              PURPOSE

sudo apt-get update 	                                   this will update all packages

sudo apt install docker.io 	                           install docker application

sudo service docker start 	                           start the docker application

systemctl status docker 	                           status of the docker application

sudo docker pull cyberxsecurity/ansible 	           download the docker file

sudo docker run -ti cyberxsecurity/ansible bash 	   run and create a docker image

sudo docker start <image-name> 	                           starts the image specified

sudo docker ps -a 	                                   list all active/inactive containers

sudo docker attach <image-name> 	                   effectively sshing into the ansible

ssh-keygen 	                                           create a ssh key

ansible -m ping all 	                                   check the connection of ansible containers
