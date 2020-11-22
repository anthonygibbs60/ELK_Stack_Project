## Automated ELK Stack Deployment
​
The files in this repository were used to configure the network depicted below.
​
![Red-Team Network](/Diagrams/Red-Team_Network_Diagram.png)
​
These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.
​
  ![install-elk.yml](Ansible/installation-elk.yml)
​
This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build
​
​
### Description of the Topology
​
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.
​
Load balancing ensures that the application will be highly secure, in addition to restricting traffic to the network.
- Load balancers play a vital role in network security by detecting and preventing distributed denial-of-service (DDoS) before they can reach your network.
- A jump box was utilized as the single access point for the network. The advantage of utilizing a jump box was ensuring only those with authorized access via the SSH key are able to access the network devices. 
​
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- The first monitoring system utilized in the ELK stack was Filebeat. Filebeat monitors and indexes log files in a centralized location. 
- The second monitoring system utilized in the ELK stack was Metricbeat. Metric beat was utilized to monitor and periodically collect key system metrics from the docker environments. 
​
The configuration details of each machine may be found below.
​
| Machine Name | Function   | Private IP Address | Public IP Address            | Operating System |
|--------------|------------|--------------------|------------------------------|------------------|
| Jump Box     | Gateway    | 10.0.0.4           | 104.40.62.127                | Linux            |
| Web 1        | Web Server | 10.0.0.5           | 13.64.152.14 (Load Balancer) | Linux            |
| Web 2        | Web Server | 10.0.0.6           | 13.64.152.14 (Load Balancer) | Linux            |
| ELK-Server   | ELK Stack  | 10.1.0.4           | 168.61.191.39                | Linux            |
​
### Access Policies
​
The machines on the internal network are not exposed to the public Internet. 
​
Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Whitelisted IP: My personal machine's IP address. 
​
Machines within the network can only be accessed by the Jump Box via SSH.
​
A summary of the access policies in place can be found in the table below.
​
| Machine Name | Publicly Accessible | Allowed IP Addresses       |
|--------------|---------------------|----------------------------|
| Jump Box     | No                  | 10.0.0.5 10.0.0.6 10.1.0.4 |
| Web 1        | No                  | 10.0.0.6                   |
| Web 2        | No                  | 10.0.0.5                   |
| ELK-Server   | No                  | 10.0.0.5 10.0.0.6          |
​
### Elk Configuration
​
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it allows administrators to automate the configuration of multiple machines added to the webservers group. 
​
The playbook implements the following tasks:
- Install docker.io 
- Install python3-pip
- Install Docker module
- Increase virtual memory and use more memory
- Download and launch a docker ELK container
​
The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![Docker ps output](docker_ps.png)
​
### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.0.0.5
- Web-2 10.0.0.6
​
We have installed the following Beats on these machines:
- Filebeat
- Metricbeat
​
These Beats allow us to collect the following information from each machine:

-Filebeat collects and visualzes system logs into a user friendly GUI in Kibana. The visualized logs allow the user to easliy monitor system logs for irregularities that deviate in the system from normal traffic seen on the network. 

![Filebeat Logs](Diagrams/filebeat.png)
​
-Metricbeat retrieves metric data from network machines such CPU and memory usage allowing users to ensure the network is running at optimal efficiency. Metricbeat can also be utilized as an IoC if irregularities are recorded in the monitored metrics.

![Metricbeat Metrics](Diagrams/metricbeat.png)

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
​
SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible.
- Update the /etc/hosts file to include the below under the # Ex 2: A collection of hosts belonging to the 'webservers' group:

  [webservers]
   10.0.0.5 ansible_python_interpreter=/usr/bin/python3
   10.0.0.6 ansible_python_interpreter=/usr/bin/python3

- Run the playbook, and navigate to 168.61.191.39:5601/app/kibana#/home to check that the installation worked as expected.
​
