## Description

### Ansible. Role dependencies+Ansible-Galaxy (TLS - NGINX+Let's+os_family Encrypt)

#### Roles description

Roles allow you to put together a playbook for different tasks from smaller parts for different situations and solutions. And you don't have to write scripts from the start.

#### Use of roles

```
- hosts:
  - all
  become: true
  become_method: sudo
          
  roles:
    - dependes
    - geerlingguy.mysql  
       
```
```
dependencies:
  - role: nginx
    vars:
      nginx_worker_processes  : "worker_processes auto;"  
      nginx_worker_connections: "worker_connections  128;"  
      nginx_sendfile          : "sendfile            off;"  
      nginx_tcp_nodelay       : "tcp_nodelay         on;"  
      nginx_tcp_nopush        : "tcp_nopush          on;"  
  - vhosts  
  - TLS  
```

### Roles ansible-galaxy geerlingguy.mysql

#### Installation  

```shell
ansible-galaxy install geerlingguy.mysql
```
#### Requirements  
No special requirements; note that this role requires root access, so either run it in a playbook with a global become: yes, or invoke the role in your playbook like:  
```
- hosts: database
  roles:
    - role: geerlingguy.mysql
      become: yes
```

#### Role Variables  
Available variables are listed below, along with default values (see defaults/main.yml):  
  
mysql_user_home: /root  
mysql_user_name: root  
mysql_user_password: root  
  
#### mysql_packages:  
  - mariadb-client  
  - mariadb-server  
  - python-mysqldb  

In [details](https://galaxy.ansible.com/geerlingguy/mysql/).   

```shell
     ansible-playbook -i ./ansible/inventories/webservers/hosts.yml ./ansible/nginx.yml --list-task      
```
```  
  play #1 (all): all  TAGS: []  
    tasks:  
      nginx : install Nginx und Letsencrypt TAGS: []  
      nginx : Remove default nginx config TAGS: [del_default_centos]  
      nginx : copy the nginx config file  TAGS: [nginx_conf]  
      vhosts : OS Vars  TAGS: []  
      vhosts : add folder for item and sertifications virtual hosts TAGS: [add_opt_folders]  
      vhosts : copy the nginx vhosts config file and the content of the web site on centos  TAGS: [erste_templates]  
      vhosts : make and copy folders and item, Check NGINX  TAGS: []  
      vhosts : Flush handlers TAGS: []  
      TLS : OS Vars_new TAGS: []  
      TLS : take cert from letsencrypt  TAGS: []  
      TLS : copy the new 443 nginx vhotsts config file on ubuntu  TAGS: [new_hosts]  
      TLS : Creation of DH (Diffie Hellman) certificate TAGS: [hellman]  
      include_tasks TAGS: []  
      geerlingguy.mysql : Check if MySQL packages were installed. TAGS: []  
      include_tasks TAGS: []  
      
``` 
  
#### Target files for role 

`./roles/{name of specific role}/{vars,tasks,handlers}/`  

## Methods of creation  

1. Provision a virtual machine using Terraform on DO and A-Record on Route 53 from AWS:  

```shell
    terraform apply -var-file=terrafor.tfvars
         or 
    terraform apply -var-file=variables.tf
```
2. Provision of virtual hosts in the cloud with the help of Ansible:

```shell
    ansible-playbook -i ./ansible/inventories/webservers/hosts.yml ./ansible/nginx.yml
```

## The results

1. After using it, you can see the results

```shell
    cat outputs_result.csv
```
**P.S. Attention! Change the values of the variables to your own!**
