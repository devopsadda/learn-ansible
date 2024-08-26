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