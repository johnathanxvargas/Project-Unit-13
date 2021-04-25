## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![ELK](./README/Images/ELK_Stack_diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the yml file may be used to install only certain pieces of it, such as Filebeat.

  - [Filebeat Playbook](./README/Files/filebeat-playbook.yml)
  - [Metricbeat Playbook](./README/Files/metricbeat-playbook.yml)
  - [ELK Playbook](./README/Files/ElkSetup.yml)

This document contains the following details:

 - Description of the Topology
 - Access Policies
 - ELK Configuration
   - Beats in Use
   - Machines Being Monitored
 - How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly redundant, in addition to restricting access to the network.
Load balancers protect overloading of the Web Servers by balancing the traffic coming to the site across multiple servers, improving redundancy and adding some prevention of total website failure.
The Jump Box helps to keep access to the Web Servers in a secured, controlled environment. By restricting backend access to the Web Servers to one machine, it is much easier to control what and who changes them as well as to determine if any malicious entities are attempting to compromise the servers.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the server and system administration.
- Filebeat sends server logs to the ELK server, and watches for outstanding changes.
- Metricbeat records information regarding the machines accessing the web server, 

The configuration details of each machine may be found below.

| Name                 | Function        | IP Address                | Operating System |
|----------------------|-----------------|---------------------------|------------------|
| Jump-Box-Provisioner | Gateway         | 52.158.230.85             | Ubuntu 18.04     |
| Web-1                | Web Server      | 10.0.0.8 / 20.191.114.40  | Ubuntu 18.04     |
| Web-2                | Web Server      | 10.0.0.9 / 20.191.114.40  | Ubuntu 18.04     |
| Web-3                | Web Server      | 10.0.0.11 / 20.191.114.40 | Ubuntu 18.04     |
| ELK-VM               | Data Monitoring | 40.87.97.160              | Ubuntu 18.04     |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

- My Home IP Address

Machines within the network can only be accessed by the Load Balancer.

The ELK Machine only allows access from the Home IP address and Load Balancer. 

A summary of the access policies in place can be found in the table below.

| Name        | Publicly Accessible | Allowed IP Addresses             |
|-------------|---------------------|----------------------------------|
| Jump Box    | Yes                 | 67.180.251.190 (Home IP Address) |
| Web-1       | No                  | 20.191.114.40 (Load Balancer)    |
| Web-2       | No                  | 20.191.114.40 (Load Balancer)    |
| Web-3       | No                  | 20.191.114.40 (Load Balancer)    |
| ELK Machine | Yes                 | 67.180.251.190 (Home IP Address) |
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because the process is sped up significantly when automated, allowing for more machines to be configured in the same amount of time.

The playbook implements the following tasks:

- Sets the amount of memory the targeted machine is configured to use
- Installs docker.io
- Installs python-3
- downloads the ELK docker container
- enables docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

! Images/ELK_docker_ps

### Target Machines & Beats
This ELK server is configured to monitor the following machines:

- 10.0.0.8
- 10.0.0.9
- 10.0.0.11

We have installed the following Beats on these machines:

- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:

- Filebeat: Filebeat collects machine logs and sends the data to Kibana, allowing one to monitor all changes to the machine. For example, a hacker changes the sudo rules on one of the web servers, allowing his own malicious account full access to the machine. Filebeat sends a log of changes to the sudo role to Kibana, and an administrator is able to determine that a change has been made. 
- Metricbeat: Metricbeat records HTTP information of machines accessing the Web Servers. For example, a man from China using MAC OS is accessing the Web Server and attempting to download system logs. Metricbeat can send all this information to Kibana, allowing someone monitoring to learn about the attempted download.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

- Copy the filebeat config file to /etc/ansible/files/filebeat-config.yml
- Update the config file to include the IP address of your ELK machine
- Run the filebeat playbook, and navigate to http://[ELK_IP]:5601/app/kibana to check that the installation worked as expected.

- Copy the metricbeat config file to /etc/ansible/files/metricbeat-config.yml
- Update the config file to include the IP address of your ELK machine
- Run the metricbeat playbook, and navigate to http://[ELK_IP]:5601/app/kibana to check that the installation worked as expected