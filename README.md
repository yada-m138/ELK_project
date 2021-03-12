## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/diagram_yada.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of **the playbook** file may be used to install only certain pieces of it, such as Filebeat.

<img width="814" alt="filebeat-int" src="Project_1/Images/filebeat-int.png">

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly **available**, in addition to restricting **access** to the network.

_What aspect of security do load balancers protect? What is the advantage of a jump box?_
- Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the **logs** and system **traffic**.

_What does Filebeat watch for?_
- Filebeat is created to watch and collect any information like logs file, locations, and events that had been changed in the system and when it changes. 

_What does Metricbeat record?_
- Metricbeat record and correct static and matric from your server then ship them into output for Elasticserch or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.5   | Linux            |
| WEb-1    | Server   | 10.0.0.6   | Linux            |
| Web-2    | Server   | 10.0.0.7   | Linux            |
| Web-3    | Server   | 10.0.0.8   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the **Jump Box** machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Host IP to Jump Box (10.0.0.5)
- Jump Box to Ansible Docker container 
- From the Jump box  container to Web-1 (10.0.0.6), Web-2 (10.0.0.7), Web-3 (10.0.0.8)
- From the Jump Box container to ELK-VM (10.1.0.4) 


Machines within the network can only be accessed by **SSH Keys**.
_Which machine did you allow to access your ELK VM? What was its IP address?_
- The only machine that allows connecting to ELK-VM is from Jump Box private IP address (10.0.0.5)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | No                  | Host IP address      |
| Web-1    | No                  | 10.0.0.5 and 10.1.04 |
| Web-2    | No                  | 10.0.0.5 and 10.1.04 |
| Web-3    | No                  | 10.0.0.5 and 10.1.04 |
| ELK-Vm   | No                  | 10.0.0.5 and Host IP |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
_What is the main advantage of automating configuration with Ansible?_
- The main advantage of automating configuration with Ansible is that you can update all the servers by running the playbook that contains all the commands that use to set up and update the server.

The playbook implements the following tasks:
_In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Create a new VM with private and public IP address
- Edit the Host file by adding an elk group after the webserver group to specify the playbook which group to run them on. Then add the ELK-sever IP address with that group we just create.
- Check your connection by ssh to your new ELK-server from your Jump Box. You can also check if your ELK playbook is installed successfully. 
- Create new Inbound Security Rules to allow Ports: 5601 and 9200 
- Open a new browser and type in the [Public IP:5601] to access the Kibana Portal Site

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Project_1/Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
_List the IP addresses of the machines you are monitoring_
- Web-1 (10.0.0.6)
- Web-2 (10.0.0.7)
- Web-3 (10.0.0.8)

We have installed the following Beats on these machines:
_Specify which Beats you successfully installed_
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
_In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Filebeat - monitors log files or locations you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat - collects metrics from the operating system and from services running on the server. Metricbeat then takes the metrics and statistics that it collects and ships them to the output that you specify.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

_SSH into the control node and follow the steps below:_
- Copy the filebeat-playbook.yml file to /etc/ansible/roles/ directory also a filebeat-config.yml file to etc/ansible/ directory.
- Update the filebeat-config.yml file to include the ELK IP address in the Elasticserch and Kibana section. 
- Run the playbook, and navigate to the ELK-VM server to check that the installation worked as expected by using docker ps command. 

_Answer the following questions to fill in the blanks:_
_Which file is the playbook? Where do you copy it?_
- The filebeat-playbook.yml is the playbook file and it’s located at /etc/ansible/roles.

_Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- First, we update the Host file by adding an ELK group after the webserver group so the playbook can understand which server needs an update. Then, we add the ELK remote_user in the ansible-config.yml file. The last file that needs to be updated is the filebeat-config.yml file by adding the ELK IP address in the Elasticsearch and Kabina sections. 
_Which URL do you navigate to in order to check that the ELK server is running?
- public ip:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
- ssh RedAdmin@10.0.0.5 > connect to Jump Box
- sudo docker container list -a > list all the container
- sudo docker start affectionate_shamir > start the container
- sudo docker attach affectionate_shamir > attach the container
- cd /etc/ansible > go to the anisble directory
- ansible-playbook int-elk.yml > start the Elk container on the Elk server.
- cd /etc/ansible/roles  > go to the roles directory 
- ansible-playbook filebeat-playbook.yml > install Filebeat 
- ansible-playbook Metricbeat-playbook.yml > install Metricbeat
- Make sure you are in the right directory before running the ansible-playbook command
