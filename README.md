Scalelite Cluster tuning playbook
=======
The goal is to create a BBB cluster with Load Balancing for Learning Platform.  
The current architecture consist of:
- 11 BBB server nodes
- 1 Scalelite + PG + Redis node
- 1 NFS node as common storage for BBB and Scalelite nodes. It will be located at AWS cloud for flexibility with hardware.
------
Software requirements
------
Ubuntu 16.04 LTS, Ansible 2.9.9, PostgreSQL 12.3, Nginx 1.16, Scalelite(images), Redis(image), NFS, Docker and Docker-compose (latest)

------
Hardware requerements
------
- BBB nodes: 4CPU, 8G RAM, 4G Swap, 200G Storage (public ip)
- NFS node: 2CPU, 4G RAM, 4G Swap, 250G Storage, 600IOPS
- Scalelite node: 4CPU, 8Gb RAM, 50G Storage (public ip)
- TURN node: 4 CPU, 8Gb RAM, 50G Storage

------
Network requirements
------
Private network: icmp, tcp, udp - no limitations
Public network: 22, 80, 443 for BBB and Scalelite nodes
- BBB nodes: opened ports (22/tcp, 80/tcp, 443/tcp, 53/tcp, 16384:32768/udp)
- NFS node: opened ports (22/tcp, 2049/tcp, 111/tcp)
- DNS A records to all BBB and Scalelite nodes
- 250Mbit/sec bandwidth
- IPv4 and IPv6

------
Install Ansible software on master host (in my case LB node)
------
It is required to install Ansible software inside the private network which has an access using ssh to database nodes. This node must have an Internet access to install software from public repositories to target nodes.
```
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt install software-properties-common -y
$ sudo apt update
$ sudo apt install ansible -y
$ sudo apt install python-psycopg2 -y
$ sudo apt install python-netaddr -y
```
How to use:
------
1. Move to /etc/ansible
2. Clone playbooks from github repo ``` git clone https://github.com/avkovalevs/scalelite_cluster.git ```
3. Add hosts, groups and ports to inventory file /etc/ansible/hosts 
4. Generate pair of rsa keys on ansible master: ``` ssh-keyget -t rsa ```
5. Copy public key to all nodes: ``` ssh-copy-id -i ~/.ssh/id_rsa.pub root@<target_host> ```
6. Check environments inside vars and default directories
7. Run playbook which deploy infrastructure ``` ansible-playbook -v -i hosts master.yml --extra-vars "env_state=present" ```
8. Check the result
------
NFS tuning steps (AWS):


Useful links:
1. https://medium.com/@jesusfederico_39370/scalelite-lazy-deployment-745a7be849f6
2. https://events.ubuntunet.net/event/29/attachments/201/246/Installing_and_Scaling_BBB.pdf
3. http://docs.bigbluebutton.org/2.2/install.html#bbb-installsh
4. https://github.com/blindsidenetworks/scalelite




