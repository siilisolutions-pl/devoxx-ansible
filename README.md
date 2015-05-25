# Introduction

This is a simple example prepared for presentation called "Ansible to rule them all" that will be given on Devoxx Poland 2015 conference. It illustrates basic usage of Ansible for provisioning simple environment and deploying Java applications.

# Environment (machines)

There are two host groups in the inventory of this example. We use Ansible to install Apache Tomcat servlet container on application servers (```appservers``` group) and deploy simple Java application build upon Spring Boot. Network traffic is routed and balanced using HAProxy software load balancer which is installed on load balancer server (```loadbalancer``` group).

Below you can see schema of machines with IP addresses and TCP connections between them.

![High-level architecture view](https://github.com/siilisolutions-pl/devoxx-ansible/blob/master/architecture.png)

# How to use it

Required software:

 * Vagrant (https://www.vagrantup.com/) - requires any virtualization software (e.g. VirtualBox) 
 * Ansible (http://ansible.com/)

You can download latest version of Vagrant from its homepage. Ansible can be installed using system package manager or ```pip```.

Required hardware:

 * Linux like operating system
 * at least 2GB of RAM (each machine from this sample uses at most 512MB of RAM).

## Start machines

To test Ansible scripts you need to start vagrant machines. We provide ```Vagrantfile``` with suitable configuration. To start all machines invoke command:
```bash
vagrant up
```

Starting machines for the first time might take few minutes, since Vagrant has to download CentOS image.

## Provision machines

Provisioning is done by Ansible. On application servers Ansible scripts install and configure Oracle Java JRE and Tomcat. Load balancers are provisioned with HAProxy. Invoke this command to provision all machines:
```bash
ansible-playbook -i inventory provisioning/site.yml --private-key=~/.vagrant.d/insecure_private_key -vv --ask-vault-pass
```

### Ansible Vault

During the provisioning you will be prompted for Ansible Vault password, type: ```vault```. It is used for decrypting ```vault.yml``` file which holds the credentials for HAProxy statistics page. You can view the statistics page by accessing following address: ```http://10.10.1.10:8080```. Default credentials are - user: ```admin``` password: ```password```.

## Deploy application

To deploy simple Java application on application servers use this command:
```bash
ansible-playbook -i deployment/site.yml --private-key=~/.vagrant.d/insecure_private_key -vv --ask-vault-pass
```

Open your web browser, application is available at ```http://10.10.1.10/```.

We provide Java application in this example in two versions: ```0.1``` and ```0.2```. The command above will deploy ```0.1``` version. To deploy version ```0.2``` you need to edit ```deployment/site.yml``` file and change value of ```app.version``` variable. Change it to ```0.2``` and invoke deployment command one more time. You should see different content when refreshing page at ```http://10.10.1.10/```.
