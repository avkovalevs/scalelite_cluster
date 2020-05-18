Scalelite Cluster tuning playbook
=============
The goal is to create a BBB cluster with Load Balancing for Learning Platform academyofmine.net 
The current architecture includes 11 BBB server nodes, 1 node with LoadBalancer Scalelite + PG + Redis, 1 node with NFS common storage for BBB nodes. 
------
Software requirements
------
Ubuntu 16.04 LTS, Ansible 2.9.9, PostgreSQL, Nginx, Scalelite, Redis, NFS, Docker 

Install Ansible software on master host
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
1. Clone playbooks from github repo
2. Add hosts to inventory file /etc/ansible/hosts
3. Generate pair of rsa keys
4. Copy public key to all nodes
5. Run playbook which deploy infrastructure
