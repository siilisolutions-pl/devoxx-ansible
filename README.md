# Introduction

This simple example was prepared for presentation "Ansible to rule them all". It illustrates basic usage of Ansible for simple environment with two application servers and one load balancer. 

# Environment (machines)

There are two host groups in the inventory of this example. On application servers (```appservers``` group) Ansible scripts install tomcat and deploy simple Java application. On load balancer servers (```loadbalancer``` group) Ansible scripts install and configure HAProxy. HAProxy is serving applications from appservers using round robin as balance algorithm.

Below you can see simple schema of machines with IP addresses and TCP connections between them. We decided to have two application servers and one load balancer server.

![High-level architecture view](https://github.com/siilisolutions-pl/devoxx-ansible/blob/master/architecture.png)

# How to use it

Required software:

 * Vagrant (https://www.vagrantup.com/)
 * Ansible (http://ansible.com/)
You can download latest version of Vagrant from its homepage. Ansible can be installed using system package manager or using ```pip```.

Required hardware:

 * Linux like operating system
 * at least 2GB of RAM (each machine from this sample uses at most 512MB or RAM).

## Start machines

To test ansible scripts you need to start vagrant machines. We provide ```Vagrantfile``` with suitable configuration. To start all machines invoke command:
```bash
vagrant up
```

## Provision machines

Provisioning is done by Ansible scripts. On application servers scripts install Oracle Java JRE and Tomcat. On load balanvers scripts install HAProxy and configure it. Invoke this command to provision all machines:
```bash
ansible-playbook -i inventory provisioning/site.yml --private-key=~/.vagrant.d/insecure_private_key -vv --ask-vault-pass
```

## Deploy application

To deploy simple Java application on application servers use this command:
```bash
ansible-playbook -i deployment/site.yml --private-key=~/.vagrant.d/insecure_private_key -vv --ask-vault-pass
```

Open your web browser, application is available at ```http://10.10.1.10/```.

We provide Java application in this example in two versions: ```0.1``` and ```0.2```. The command above will deploy ```0.1``` version. To deploy version ```0.2``` you need to edit ```deployment/site.yml``` file and change value of ```app.version``` variable. Change it to ```0.2``` and invoke deployment command one more time. You should see different content when refreshing page at ```http://10.10.1.10/```.
