# Ansible-tutorial

This tutorial will guide you through the process of setting up Ansible and using it to automate tasks on multiple EC2 instances. We'll start by creating three EC2 instances - one to act as the Ansible host, and two to serve as Ansible servers. Then, we'll install Ansible, copy AWS keys, and perform basic ad-hoc commands. Finally, we'll delve into the usage of playbooks to automate more complex tasks like installing Nginx, deploying a web page, and updating its content.

## Prerequisites

Before proceeding, ensure you have the following:

- An AWS account with necessary permissions to create EC2 instances and manage security groups.
- SSH key pair generated for accessing EC2 instances.
- Watch this amazing [video](https://www.youtube.com/watch?v=1id6ERvfozo) by [TechWorld with Nana](https://www.youtube.com/@TechWorldwithNana)

## Step 1: Create EC2 Instances

Create three EC2 instances with the following roles:

1. Ansible Host
2. Ansible Server 1
3. Ansible Server 2

## Step 2: Install Ansible and modifying the /etc/ansible/hosts

1. Connect to the Ansible host instance via SSH.
2. Install Ansible using your system's package manager.
   ```
   sudo apt-add-repository ppa:ansible/ansible
   ```
   ```
   sudo apt update
   ```
   ```
   sudo apt install ansible
   ```
3. Modify the /etc/ansible/hosts

   ```
   sudo vim /etc/ansible/hosts
   ```

   ```
   [servers]

   server1 ansible_host=PUBLIC_IPV4
   server2 ansible_host=PUBLIC_IPV4

   [servers:vars]
   ansible_python_interpreter=/usr/bin/python3
   ansible_ssh_private_key_file=/home/ubuntu/.ssh/<AWSKey>
   ```

## Step 3: Set Up SSH Keys

1. Copy the AWS key pair to the Ansible host:
   ```
   scp -i "<key_pair_name>" <key_pair_name> ubuntu@<public_DNS_of_EC2>:/home/ubuntu/.ssh
   ```
2. Change the permissions of the copied key:
   ```
   chmod 400 /home/ubuntu/.ssh/<key_pair_name>
   ```
3. SSH into the EC2 instances check the key integrity and add EC2 instances to the known hosts.

## Step 3: Now you can perform some Ad-hoc Commands

Ad hoc commands are one-off commands that are executed on the command line of an Ansible control node, without the need for a playbook or any additional configuration

1. Check connectivity:

   ```
    ansible all -m ping
   ```

2. Update packages
   ```
   ansible servers -a "sudo apt update"
   ```
3. Try running some more commands!

## Step 4: Using Playbooks

Playbooks provide a way to define and execute Ansible tasks in a more structured manner. Let's create playbooks for installing Nginx, deploying a web page, and updating its content.

1. Installing Nginx
   You can find the yaml code in install_nginx.yml file
   Run the playbook:

```
ansible-playbook install_nginx.yml
```

2. Deploying a Webpage
   You can find the yaml code in deploy_webpage.yml file
   Run this playbook similarly

3. Update the Webpage
   You can find the yaml code in update_webpage.yml file
   Run this playbook similarly

## Accessing Web Page

To view the deployed web page, access the public IP of any EC2 server. Ensure that inbound rules for the EC2 instances allow traffic on port 80 (HTTP).

## Conclusion

Congratulations! You have successfully set up Ansible, executed ad-hoc commands, and automated tasks using playbooks. You can further explore Ansible's capabilities to streamline your infrastructure management and deployment processes.
