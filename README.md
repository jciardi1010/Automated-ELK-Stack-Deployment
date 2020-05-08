## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/jciardi1010/Automated-ELK-Stack-Deployment/blob/master/Diagrams/Virtual%20Network%20Architecture.PNG

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above or certain files can be downloaded such as Filebeat via the filebeatplaybook.yml file.

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly stable, in addition to restricting public access to the network. The load balancer ensures that the VM’s that we have do not get overloaded due to traffic or any (minor) attempts at DDoS attacks. The jump box that we set up prevents unauthorized users from accessing our VM’s through undesired methods (ssh, for example).


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and files via Filebeat and system and service statistics via Metricbeat.

The configuration details of each machine may be found below.
| Name       | Function          | Public IP       | Private IP  | Operating System|
|------------|-------------------|-----------------|-------------|-----------------|
| Jump Box   | Gateway           | 104.42.169.126  | 10.1.0.4    | Linux/Ubuntu    | 
| DVWA1      | Web App           | 104.42.38.81    | 10.1.0.5    | Linux/Ubuntu    |
| DVWA2      | Web App           | 104.42.2.250    | 10.1.0.7    | Linux/Ubuntu    |
| ELK Server | Monitoring Server | 168.62.176.251  | 10.0.0.4    | Linux/Ubuntu    |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the load balancer can accept connections from the Internet. Access to this machine is only allowed from the VM’s on my virtual network. In this scenario, that would be:
| VM       | Private IP addr |
|----------|-----------------|
| DVWA-VM1 | 10.1.0.5        |
| DVWA-VM2 | 10.1.0.7        |

Machines within the network can only be accessed by the jump box.
Jump Box 10.1.0.4

A summary of the access policies in place can be found in the table below.
| Name            | Publicly Accessible   | Allowed IP addresses                                                                    |
|-----------------|-----------------------|-----------------------------------------------------------------------------------------|
| Jump Box        | No                    | Personal Public IP (Port 22)                                                            |
| DVWA1           | No                    | 10.1.0.4 (Port 22), 138.91.73.244                                                       |
| DVWA2           | No                    | 10.1.0.4 (Port 22), 138.91.73.244                                                       |
| ELK Server      | No                    | 104.42.169.126 (Port 22), Personal Public IP (Port 5601), 104.42.38.81 (Port 5601, 9200)|   |    (cont.)                                 104.42.2.250 (Port 5601, 9200)138.91.73.244 (Port 9200)                                 |
| Load Balancer   | Yes                   | Internet (Port 80)                                                                      |

### Elk Configuration

Ansible was used to automate the configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows the user to deploy complex architecture to as many systems as needed very easily by eliminating redundancy.

The playbook implements the following tasks:
- Installs Docker
- Downloads the required images
- Installs the configurations to the ELK server
- Installs the Filebeat configurations to the DVWA servers

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/jciardi1010/Automated-ELK-Stack-Deployment/blob/master/Diagrams/Docker%20ps.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
DVWA-VM1 104.42.38.81
DVWA-VM2 104.42.2.250

We have installed the following Beats on these machines:
filebeat-7.4.0-amd64.deb
(*metricbeat was listed as a bonus but I didn’t get to it)

These Beats allow us to collect the following information from each machine:
Filebeat is used to format log files and has the ability to forward this data elsewhere to be processed and analyzed (such as an ELK server in this scenario). This data will contain information such as event logs to track the activity on the system.
Metricbeat is used to collect system and server metrics such as CPU and memory which can then be sent to another server to be processed and analyzed (such as an ELK server). This data will contain information such as memory usage and how much free space is available.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to the /etc/ansible folder.
- Copy the filebeatconfig.yml file to the /etc/ansible/files folder. Be sure to have the IP addresses in this file set to your ELK server public IP and port 5601.
- Copy the filebeatplaybook.yml file to the /etc/ansible/roles folder.
- Update the /etc/ansible/hosts file to include a webservers section and an ELK servers section. From here, edit this file to include the IP addresses of the respective VM’s. In this scenario, I’ve added the 2 DVWA VM IP addresses under “webservers” and the ELK server IP address under “elkservers”.

- Run the filebeatplaybook.yml file in the https://github.com/jciardi1010/Automated-ELK-Stack-Deployment/tree/master/Ansible folder to install Filebeats on your webservers.
- Run the Install-elk.yml playbook, and navigate to the ELK server public IP in your web browser address bar (including the port), in my case that would be http://168.62.176.251:5601/, to check that the installation worked as expected.


