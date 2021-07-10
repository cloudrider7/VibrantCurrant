## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](https://github.com/cloudrider7/VibrantCurrant/blob/main/Diagrams/Cloud%20Security.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  - [View the associated filebeat playbook file](https://github.com/cloudrider7/VibrantCurrant/blob/main/Ansible/filebeat_playbook.yml)

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
- The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. Jump boxes prevent less secure machines from being able to be exploited with elevated privileges.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.
- Filebeat monitors the log files or locations that you specify, collects log events, and forwards them either to Elasticsearch or Logstash for indexing.
- Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

The configuration details of each machine may be found below.

|        Name        |  Function  | IP Addr  |   OS  |
|--------------------|------------|----------|-------|
| JumpBoxProvisioner |   Gateway  | 10.1.0.4 | Linux |
|        Web1        | Web Server | 10.1.0.5 | Linux |
|        Web2        | Web Server | 10.1.0.6 | Linux |
|      DWVA-VM3      | Web Server | 10.1.0.9 | Linux |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBoxProvisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following CIDR rage:
- 37.19.211.0/24

Machines within the network can only be accessed by the JumpBoxProvisioner machine.  The JumpBoxProvisioner machine was also the only internal machine allowed to access the ELKStack1 machine, via its public IP address 20.85.219.252.

A summary of the access policies in place can be found in the table below.

|        Name        | Accessible |   Allowed IPs  |
|--------------------|------------|----------------|
| JumpBoxProvisioner |    Yes     | 37.19.211.0/24 |
|        Web1        |     No     |    10.1.0.4    |
|        Web2        |     No     |    10.1.0.4    |
|      DWVA-VM3      |     No     |    10.1.0.4    |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because having done so would have resulted in a lot of tedious and repetitive work.

The playbook implements the following tasks:
- Installs docker.io and the Python package manager, pip3;
- Automatically resizes VRAM to support the ELK stack;
- Installs and deploys the relevant ELK container; and
- Enables docker service to run on boot.

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.1.0.5
- 10.1.0.6
- 10.1.0.9

We have installed the following Beats on these machines:
- Filebeat; Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat records logs from specified locations, and metricbeat records statistics from specified services.  We expect to see any unauthorised access attempts captured in logs from filebeat, and statistics such as unique visior count collected by metricbeat.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-configuration.yml file to /etc/ansible/roles/files.
- Update the filebeat-configuration.yml file to include the ELK private IP in lines 1106 and 1806.
- Run the playbook, and navigate to the ELKStack1 VM via its public IP address at http://20.82.73.181:5601/ to check that the installation worked as expected.

- The playbook file is called filebeat_playbook.yml, and can be found in /etc/ansible/roles.
- Update the ansible hosts file (ansible.cfg, found in /etc/ansible/hosts) to run the playbook on a specific machine.   In the hosts file, you can specify separate groups, such as [[webservers]] and [[elkservers]], to stipulate whether or not to add additional services, such as Filebeat.
- Navigate to http://20.82.73.181:5601/ in order to check that the ELK server is running.