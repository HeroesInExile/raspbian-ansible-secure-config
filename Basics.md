# Ansible Basics
Follow along with Jeff Geerling building PiDramble with Ansible
###https://youtu.be/ZNB1at8mJWY###
####################################################
# inventory file 
#ansible looks for /etc/ansible/hosts
####################################################
[webservers]
host1
host2
10.0.1.23

[workstations]
host3

[database]
10.0.1.26

[collection:children]
webservers
workstations
database
[collection:vars]
ansible_ssh_user=pi
####################################################
# ad-hoc commands or modules on the servers
####################################################
Format:
ansible group -m module -a arguments
#for sudo: -s or --sudo

#ping module
ansible collection -m ping

#change RBG led on webservers to blue
ansible webservers -a "rgb blue" -s


#ansible, by default, runs many connections, if we want single connections:
ansible all -a "rgb blue" -s --forks=1
#memory usage on all servers
ansible all -a "free -m"
#setup module
ansible all -m setup

#get all ips on servers, using shell module allows piping
ansible all -m shell -a "ifconfig | grep inet" -s

#change services on all servers balanced
ansible webservers -m service -a "name-nginx state-restarted" -s --forks=2

####################################################
# playbooks on the servers
####################################################
#to run the playbook
ansible-playbook playbook.yaml
#YAML for NGinx webservers
<#
---
- hosts: webservers
  sudo: yes
  serial: 2 

  vars:
    nginx_listen_ports:80

  pre_tasks:
   -name: Indicate Ansible is deploying to the server.
    command: /usr/bin/rgb red
    changed_when: false

  tasks:
    -name: Ensure Myapp user account is present.
     user: name=myapp shell=/bin/bash groups=sudo state=present

    -name: Ensure NGinx is installed.
     apt: name=nginx state=installed

    -name: Ensure NGinx example config is present.
     template: src=templates/example.j2.conf dest=/tmp/example.conf mode=0644
     notify: reload nginx

  post_tasks:
    -name: Indicate Ansible is finished deploying to the server.
     command: /usr/bin/rgb green
     changed_when: false

    handlers:
    -name: reload nginx
    service: name=nginx state=reloaded

#>
####################################################
# Example Config Template----example.j2.conf
####################################################
server{
    listen {{ nginx_listen_port }};
    root /var/www/html;

    location / {
    }
}

