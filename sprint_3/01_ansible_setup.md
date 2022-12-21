# Ansible

> Ref:
> * [ansible-ec2-demo](https://github.com/pasc-ed/ansible-ec2-demo)
> * [ansible-playbook-demo](https://github.com/pasc-ed/ansible-playbook-demo)

## Setup

Install ansible using pip:
```sh
pip install ansible
```

Create a new folder or download a new repository from your GitHub account:
```sh
cd ~/Documents/Talent-Academy
mkdir ansible-demo
```
or 
```sh
git clone git@github.com:aaaaaaa/ansible-demo.git
```

Move into your folder
```sh
cd ansible-demo
```

### Set up ansible.cfg

To set up basics configurations in your ansible project, create a new file named `ansible.cfg` and add the following lines in the file. Read more about the different options from the [config documentation](https://docs.ansible.com/ansible/latest/reference_appendices/config.html).
```sh
[defaults]
# Prevent SSH host key check when connecting to target machine
host_key_checking = False
# Inventory file location in the folder
inventory=./inventory.hosts
# Avoid creating of retry files if ansible fails
retry_files_enabled = False
# Color in TTY
force_color = 1
# main callback used to display Ansible output
stdout_callback = debug
# disabling of warnings related to potential issues on the system
system_warnings = False
# showing of deprecation warnings
deprecation_warnings=False
```

### Inventory file

The Basic inventory file will have the group of servers, listed by their ip addresses. Create a new file called `inventory.hosts` and add your server ip, for example as a `webservers`.

```sh
[webservers]
192.168.1.11
192.168.1.12
192.168.1.13
```

You can also add variables attached to your group of servers, for example the user to connect to and the SSH private key location. Edit your `inventory.hosts` file:
```sh
[webservers]
192.168.1.11

[webservers:vars]
ansible_user = "ec2-user"
ansible_ssh_private_key_file = "~/.ssh/ta-lab-ansible-server"
```

## Usage

### Ad hoc command

A simple use of using ansible is to call the commande directly from your terminal by specifying the group of server to connect and the module to use, example the [ping module](https://docs.ansible.com/ansible/2.9/modules/ping_module.html).

```sh
ansible webservers -m ping
```

This is using SSH to connect to the target servers, instead of a normal **ICMP ping**.

### Playbook

Using a playbook allows you to list all the tasks to be executed on the remote target server. Set up the playbook to define:
* the group to execute the tasks on
* disable facts gathering (if not required)
* listing the tasks

Create a new file named `playbook.yml`:
```yaml
---
- hosts: webserver
  gather_facts: no
  tasks:
    - name: Creating my new folder
      file:
        path: "{{ my_demo_folder_path }}"
        state: directory
        owner: ec2-user

    - name: Creating new file
      file:
        path: "{{ my_demo_folder_path }}/new_file.txt"
        state: touch
```

### Variable file

Create a new variable file dedicated to the group of servers. For that, first create a new folder:
```sh
mkdir group_vars
```

Inside the folder, create the group file with the same name:

```sh
vi group_vars/webservers.yml
```

```yaml
my_demo_folder_path: "/home/ec2-user/my_new_folder"
```

### Run playbook

Run your playbook using the following command:
```sh
ansible-playbook playbook.yml
```