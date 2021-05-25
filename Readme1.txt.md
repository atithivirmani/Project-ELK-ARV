
# Project-ELK-ARV
**\## Automated ELK Stack Deployment**

The files in this repository were used to configure the network depicted below.
![ELK Network Diagram](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Diagrams/ELKNetworkDiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the file may be used to install only certain pieces of it, such as Filebeat.

[Configure ELK VM with Docker](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Ansible/Configure%20ELK%20VM%20with%20Docker.yml.txt)

[Configure Web VM with Docker](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Ansible/Configure%20Web%20VM%20with%20Docker.yml.txt)

[Configure FileBeat](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Ansible/Filebeat-Playbook.yml.txt)

[Configure MetricBeat](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Ansible/Metricbeat-Playbook.yml.txt)

This document contains the following details:

    - Description of the Topology
    - Access Policies
    - ELK Configuration
    - Beats in Use
    - Machines Being Monitored
    - How to Use the Ansible Build


**\# Description of the Topology**

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the *Damn Vulnerable Web Application.*

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.

    - Load balancers protect systems from DDoS attacks like SYN flood by shifting traffic and improving the resiliency of systems.

    The advantage of a jump box is to give access to user from a single node that can be secured and monitored.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.


- Metricbeat takes the metrics and statistics that it collects and ships them to the output that is specified, such as Elasticsearch or Logstash

The configuration details of each machine may be found below.

|Name |Function  |IP Address |Operating System  |
|--|--|--|--|--|--|--|--|
| Jump Box |Gateway|10.0.0.1 |Linux  |
| Web 1 |Web server|10.0.0.8 |Linux  |
| Web 2 |Web server|10.0.0.9 |Linux  |
| ELK |ELK Server|10.2.0.4 |Linux  |
| LB |Load Balancer|191.239.178.214 |Linux  |
| User System|Access Control  |14.201.15.150 |Linux  |
| Web 3 |Web server|10.0.0.13 |Linux  |

**\# Access Policies**

The machines on the internal network are not exposed to public Internet.

Only the Elk Server machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

    - 14.201.15.150 over Port 5601

Machines within the network can only be accessed by Workstation and Jump-Box-Provisioner.

    - Which machine did you allow to access your ELK VM? What was its IP address?
    
    Jump-Box-Provisioner IP : 10.0.0.7 via SSH port 22
    
    Workstation Public IP :  14.201.15.150 via port TCP 5601

A summary of the access policies in place can be found in the table below.

|Name|Publicly Accessible  |Allowed IP Addresses|
|--|--|--|--|--|--|--|--|
| Jump Box |No|14.201.15.150 on SSH 22
| Web 1 |No |10.0.0.8 on SSH 22
| Web 2 |No |10.0.0.9 on SSH 22
| Web 3 |No |10.0.0.13 on SSH 22
| ELK Server |No |14.201.15.150 using TCP 5601
| Load Balancer|No |14.201.15.150 on HTTP 80


**\# ELK Configuration**

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

  

  - Ansible lets you quickly and easily deploy multi-tier apps. You won't need to write custom code to automate your systems; you list the tasks required to be done by writing a playbook, and Ansible will figure out how to get your systems to the state you want them to be in.

The playbook implements the following tasks:

 - installs docker.io, pip3, and the docker module.
    

>         >   # Use apt module
>     >       - name: Install docker.io
>     >           apt:
>     >             update_cache: yes
>     >             name: docker.io
>     >             state: present

    
      # Use apt module
>      - name: Install pip3
>               apt:
>                 force_apt_get: yes
>                 name: python3-pip
>                 state: present

    
      # Use pip module
>      - name: Install Docker python module
>           pip:
>             name: docker
>             state: present

 - increases the virtual memory (for the virtual machine we will use to run the ELK server)

>      # `Use command module`
> 
>     >     - name: Increase virtual memory
>     >       command: sysctl -w vm.max_map_count=262144

 - uses sysctl module

>      # Use sysctl module
>         - name: Use more memory
>           sysctl:
>             name: vm.max_map_count
>             value: "262144"
>             state: present
>             reload: yes

 - downloads and launches the docker container for elk server

>     # Use docker_container module
>         - name: download and launch a docker elk container
>           docker_container:
>             name: elk
>             image: sebp/elk:761
>             state: started
>             restart_policy: always
>             published_ports:
>               - 5601:5601
>               - 9200:9200
>               - 5044:5044

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![ELK Running](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Images/ELK%20running.PNG)

*The status should be up*.


**\# Target Machines & Beats**

This ELK server is configured to monitor the following machines:

    Web1: 10.0.0.8
    
    Web2: 10.0.0.9
    
    Web3: 10.0.0.13

We have installed the following Beats on these machines:

 - Filebeat
 - Metricbeat

These Beats allow us to collect the following information from each machine:

 - Filebeat is a log data shipper for local files. Installed as an agent on your servers, Filebeat monitors the log directories or specific log files, tails the files, and forwards them either to Elasticsearch or Logstash for indexing. An examle of such are the logs produced from the MySQL database supporting our application.

![Filebeat: log events](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Diagrams/Filebeat.PNG)

 - Metricbeat collects metrics and statistics on the system. An example of such is cpu usage, which can be used to monitor the system health.
 
![Metricbeat: metrics and system statistics](https://github.com/atithivirmani/Project-ELK-ARV/blob/main/Diagrams/Metricbeat.PNG)


**\#Using the Playbook**

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

For ELK VM Configuration:

- Copy the Ansible ELK Installation and VM Configuration
- Run the playbook using this command : ansible-playbook install-elk.yml

**For FILEBEAT:**

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
          src: /etc/ansible/files/filebeat-config.yml
          dest: /etc/filebeat/filebeat.yml
    
        # Use command module
      - name: Enable and Configure System Module
        command: filebeat modules enable system
    
        # Use command module
      - name: Setup filebeat
        command: filebeat setup
    
        # Use command module
      - name: Start filebeat service
        command: service filebeat start

For METRICBEAT:

    - name: Install metric beat
      hosts: webservers
      become: true
      tasks:
        # Use command module
      - name: Download metricbeat
        command: curl -L -O https://artifacts.elastic.co/downloads/beats/metricbeat/metricbeat-7.4.0-amd64.deb
    
        # Use command module
      - name: install metricbeat
        command: dpkg -i metricbeat-7.4.0-amd64.deb
    
        # Use copy module
      - name: drop in metricbeat config
        copy:
          src: /etc/ansible/files/metricbeat-config.yml
          dest: /etc/metricbeat/metricbeat.yml
    
        # Use command module
      - name: enable and configure docker module for metric beat
        command: metricbeat modules enable docker
    
        # Use command module
      - name: setup metric beat
        command: metricbeat setup
    
        # Use command module
      - name: start metric beat
        command: service metricbeat start

Answer the following questions to fill in the blanks:\_

 - Which file is the playbook? Where do you copy it?

    For ANSIBLE : We will create the my-playbook1.yml as our
       playbook.
       
 

      For FILEBEAT: We will create filbeat-playbook.yml as our playbook.
       
   

    For METRICBEAT: We will create metricbeat-playbook.yml as our
       playbook.

 - Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?

You need to update the hosts file within /etc/ansible to run the playbook on a specific machine

You create a [elk] group in /etc/ansible/hosts for VM on which to run ELK. You specify the VM IP in FileBeat config file on which to install Filebeat.

- Which URL do you navigate to in order to check that the ELK server is running?

Test Kibana on web : http://[your.ELK-VM.External.IP]:5601/app/kibana

Test Kibana on localhost: sysadmin@10.2.0.4: curl localhost:5601/app/kibana


As a \*\*Bonus\*\*, provide the specific commands the user will need to run to download the playbook, update the files, etc.

|Command|Purpose  |
|--|--|
| sudo apt-get update | this will update all packages |
| sudo apt install docker.io  | this will update all packages |
| sudo service docker start | install docker application |
| systemctl status docker | start the docker application |
| sudo docker pull cyberxsecurity/ansible | status of the docker application |
| sudo docker run -ti cyberxsecurity/ansible bash | run and create a docker image |
| sudo docker start <image-name> | starts the image specified |
| sudo docker ps -a  | list all active/inactive containers |
| sudo docker attach <image-name> | effectively sshing into the ansible |
| ssh-keygen | create a ssh key |
| ansible -m ping all | check the connection of ansible containers |
