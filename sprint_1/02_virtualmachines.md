# Virtual Machines with Vagrant

## Your Mission
As the ancestor to the Cloud, we'll start deploying virtual machines on our own mac and work through some basic commands and networking. Vagrant is a very easy tool which will allow us to configure our VMs easily and quickly. This is very useful specially when (mostly if) something breaks within the VM and is unrecoveerable. We can just easily destroy it and rebuilt a new one using Vagrant and have it configured as it was initially.

For this workshop, should you choose to accept it, you will be deploying 2 virtual machines:
- VM-1 : Linux Web server
- VM-2: Windows Client

You will need to make sure that both servers can communicate to each other and analyse the packets that they are exchanging between them using `wireshark` and `tcpdump`.

You will also go through a few basic Windows and Linux Command Line to familiarize yourself with it.

As always, should any of your VMs gets misconfigured or destroyed ... just re-run the Vagrant config file. This file will not self-destruct anytime soon.

Good luck.

---
## Get Started

- **Step 1 : Install Vagrant**

```sh
brew install vagrant

# Check version of vagrant
vagrant --version
```

> :warning: **Kernel Error**
    *Mac requires some permission to access the kernel driver, follow [this link](https://www.howtogeek.com/658047/how-to-fix-virtualboxs-%E2%80%9Ckernel-driver-not-installed-rc-1908-error/), if you get a `Kernel Driver Not Installed` error. A **restart** of your macbook will be required.*

- **Step 2 : working dir**

Create a local directory for Vagrant configuration files

```sh
mkdir vm_workshop
cd vm_workshop
mkdir web-server win-client
```

- **Step 3 : Shared directory**

Also create a folder on your mac that will act as a shared folder between the host(your mac) and the VM. All the content of the folder will be visible on the virtual machine.

```sh
# create folder
mkdir ~/Documents/shared
```

---

## Create Vagrantfiles

Vagarnt allows you to deploy Virtual Machines from images of various operating system. Go to the [vagrant cloud](https://app.vagrantup.com/boxes/search) to have a list of available images.

- **Step 4 : Web Server VM**

In the  `web-server folder` create a new `Vagrantfile` and copy-paste the code below. Don't forget to change the `MAC_USER` variable to properly reference your shared folder.

```sh
cd web-server
touch Vagrantfile
```

```ruby
# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :


# VM box image
VAGRANT_BOX = 'ubuntu/trusty64'
# Memorable name for your
VM_NAME = 'web-server'
# VM User — 'vagrant' by default
VM_USER = 'vagrant'   
# Username on your Mac
MAC_USER = 'EthanHunt'
# Host folder to sync
HOST_PATH = '/Users/' + MAC_USER + '/Documents/shared/'  
# Where to sync to on Guest — 'vagrant' is the default user name
GUEST_PATH = '/home/' + VM_USER + '/' + VM_NAME

# Vagrant box from Hashicorp
Vagrant.configure(2) do |config|
  config.vm.box = VAGRANT_BOX
  
  # Actual machine name
  # Set VM name in Virtualbox
  config.vm.hostname = VM_NAME
  config.vm.provider "virtualbox" do |v|
    v.gui = true
    v.name = VM_NAME
    v.memory = 4096
  end
  
  # PRIVATE NETWORK — set private network with static ip
  config.vm.network "private_network", ip: "192.168.50.10",
    virtualbox__intnet: "private_network"

  # Disable default Vagrant folder, use a unique path per project
  config.vm.synced_folder HOST_PATH, GUEST_PATH
  config.vm.synced_folder '.', '/home/' + VM_USER + '', disabled: true 
  # Install apache2
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y git
    apt-get upgrade -y
    apt-get autoremove -y
    apt-get install apache2 -y
  SHELL
end
```

Run vagrant command to deploy the VM:

```sh
vagrant up
```

You can now connect to the VM with:
```sh
vagrant ssh
```

Check if the service is running using the following command, inside the virtual machine. You should have a result like below :

```sh
vagrant@web-server:~$ service apache2 status
 * apache2 is running
```

or, with the command:

```sh
curl -I http://localhost
```

Example: 
```sh
vagrant@web-server:~$ curl -I http://localhost
HTTP/1.1 200 OK
Date: Tue, 09 Nov 2021 20:51:41 GMT
Server: Apache/2.4.7 (Ubuntu)
Last-Modified: Tue, 09 Nov 2021 20:43:26 GMT
ETag: "2cf6-5d06129482f8b"
Accept-Ranges: bytes
Content-Length: 11510
Vary: Accept-Encoding
Content-Type: text/html
```

- **Step 5 : Win Client VM**

Same step for the `Windows server 2022` box. Create a new `Vagrantfile` inside the `win-client` folder this time. Again, don't forget to change the `MAC_USER` variable to properly reference your shared folder.

```ruby
# encoding: utf-8
# -*- mode: ruby -*-
# vi: set ft=ruby :

# VM box image
VAGRANT_BOX = 'gusztavvargadr/windows-server'
# Memorable name for your
VM_NAME = 'win-client'
# VM User — 'vagrant' by default
VM_USER = 'vagrant'   
# Username on your Mac
MAC_USER = 'EthanHunt'
# Host folder to sync
HOST_PATH = '/Users/' + MAC_USER + '/Documents/shared/'   
# Where to sync to on Guest — 'vagrant' is the default user name
GUEST_PATH = '/home/' + VM_USER + '/' + VM_NAME

# Vagrant box from Hashicorp
Vagrant.configure(2) do |config|
  config.vm.box = VAGRANT_BOX
  
  # Actual machine name
  config.vm.hostname = VM_NAME
  config.vm.provider "virtualbox" do |v|
    v.gui = true
    v.name = VM_NAME
    v.memory = 4096
  end
  # PRIVATE NETWORK — set private network with static ip
  config.vm.network "private_network", ip: "192.168.50.20",
    virtualbox__intnet: "private_network"

  # Disable default Vagrant folder, use a unique path per project
  config.vm.synced_folder HOST_PATH, GUEST_PATH
  config.vm.synced_folder '.', '/home/' + VM_USER + '', disabled: true  
end
```
---

- **Step 6 : Windows server Firewall**

Go to Firewall and enable Inbound rules to accept ping.

Perform a ping between the servers, to make sure they can communicate between each other.

- **Step 7 : Windows - install Wireshark**

Download and install the wireshark.