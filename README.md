# Ansible Learning Materials

# Learning notes - concepts

## Introduction, Configuration and Basic tasks.
### Why Ansible?
- Provisioning
- Configuration Management
- Continuous Delivery
- Application Deployment
- Secuirty Compliance


## Configuration Files

```shell

/etc/ansible/ansible.cfg
[defaults]
inventory           = /etc/ansible/hosts

[inventory]

[priviledge_escalation]

[ssh_connection]

```

## Ansible Inventory
```shell
/etc/ansible/hosts

[web]
ansible_host=server1.example.com ansible_connection=ssh ansible_user=root
server2.example.com
[app]
server3.example.com
server4.example.com
[db]
server5.example.com
server6.example.com

```
### Ansible INI Format
```shell
[webservers]
web1.example.com
web2.example.com

[dbservers]
db1.example.com
db2.example.com
```

### Ansible YAML Format
```shell
all:
  children:
    webservers:
      hosts:
        web1.example.com:
        web2.example.com:
    dbservers:
      hosts:
        db1.example.com:
        db2.example.com:
```

## Ansible Variables and Facts
- Variables stores information that varies with each host.
- Jinja2 templating 
- Precedence
  ○ Group Vars
  ○ Host Vars
  ○ Host Facts
  ○ Play Vars
  ○ Role Vars
  ○ Include Vars
  ○ Set Facts
  ○ Extra facts
### Variable Scope

## Ansible Playbooks
![alt text](image.png)

### What is Playbook?
- Playbook - A playbook YAML file (playbook.yml)
    - Play - Defines a set of activities(tasks) to be run on hosts
        - Task - An action to be performed on the host
            - Execute a command
            - Execute a script
            - Install a package
            - Shutdown/Restart
```shell
---
- name: Samnple Playbook 
  hosts: localhost
  tasks:
    - name: Execute command 'date'
      command: date

    - name: Execute script on the server
      script: test_script.sh

    - name: Install httpd service
      yum:
        name: httpd
        state: present
    
    - name: Start web server
      service: 
        name: httpd
        state: started
```
### How to varify Playbooks in Ansible?
```shell
ansible-playbook install_nginx.yml --syntax-check
ansible-playbook install_nginx.yml --check
ansible-playbook install_nginx.yml --check --diff
```

### Ansible lint (ansible-lint)
```shell
ansible-lint style_playbook.yml
```

# Hands-On Labs
mater-node : This host will act as an Ansible master node where you will create playbooks, inventory, roles etc and you will be running your playbooks from this host itself.

node01: This host will act as an Ansible client/remote host where you will setup/install some stuff using Ansible playbooks. Below are the SSH credentials for this host:
username: bob, password: caleston123

node02: This host also will act as an Ansible client/remote host where you will setup/install some stuff using Ansible playbooks. Below are the SSH credentials for this host:
username: bob, password: caleston123
## Example Playbook 01: How ansible plays are there in the playbook?

```yaml
---
- name: Setup apache
  hosts: webserver
  tasks:
    - name: install httpd
      yum:
        name: httpd
        state: installed
    - name: Start service
      service:
        name: httpd
        state: started

- name: Setup tomcat
  hosts: appserver
  tasks:
    - name: install httpd
      yum:
        name: tomcat
        state: installed
    - name: Start service
      service:
        name: tomcat
        state: started
```
## Example Playbook 02: How many tasks are there under the Setup Apache Ansible play?


```yaml
---
- name: Setup apache
  hosts: webserver
  tasks:
    - name: install httpd
      yum:
        name: httpd
        state: installed
    - name: Start service
      service:
        name: httpd
        state: started

- name: Setup tomcat
  hosts: appserver
  tasks:
    - name: install httpd
      yum:
        name: tomcat
        state: installed
    - name: Start service
      service:
        name: tomcat
        state: started
```

# POC Solutions/Projects

# Interview Q/A

Scenario 01: Suppose you have a requirement to store the output of one task to use it later.
Ex: Capture the output of first command and pass it to second command.

```shell
---
- name: Check /etc/hosts file
  hosts: all
  tasks:
  - shell: cat /etc/hosts
    register: result

  - debug:
    var: result




```