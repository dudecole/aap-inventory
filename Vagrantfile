# TODO:
# 1. configure rhel8 repos using iso or html source for 'BaseOS' and 'AppStream'
# 	so ansible AAP and python packages can be installed correctly.
# 	If this is not set then there will be issues installing ansible AAP on RHEL8
# 2. create ssh keys for root on the controller, and copy them to all cluster nodes

# -*- mode: ruby -*-
# vi: set ft=ruby :

##################################
##### START OF GITLAB SCRIPT
##################################
#Taken from: https://about.gitlab.com/install/#centos-7
$gitlab_config = <<-SCRIPT

yum update -y 
yum install -y wget python3 vim 

python3 -m pip install --upgrade pip 

python3 -m pip install ansible --force-reinstall

# SELinux
setenforce 0

# change ssh settings
sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
sed -i 's/^PasswordAuthentication no/#PasswordAuthentication no/g' /etc/ssh/sshd_config
sed -i 's/^#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd

# add gitlab repo
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ee/script.rpm.sh | sudo bash

# get IP address of host and assign to variable
#ip a | 

# set URL and install gitlab EE
sudo EXTERNAL_URL="http://192.168.56.11" yum install -y gitlab-ee
#
## query the password at:
cp /etc/gitlab/initial_root_password ~/gitlab-root-password.txt


systemctl status firewalld
systemctl disable firewalld
systemctl stop firewalld

# disable subscription manager to test out epel repos w/ ansible
# subscription-manager config --rhsm.auto_enable_yum_plugins=0


# Disable SELinux
setenforce 0

SCRIPT
##################################
## END OF GITLAB SCRIPT
##################################


##################################
##### START OF CentOS controller SCRIPT
##################################
$controller_config = <<-SCRIPT

yum install -y wget python3 vim 

python3 -m pip install --upgrade pip 

python3 -m pip install ansible --force-reinstall

useradd -m -d /home/ansible-admin -s /bin/bash ansible-admin
mkdir -pv /home/ansible-admin/.ssh/ && \
 ssh-keygen -t rsa -f /home/ansible-admin/.ssh/id_rsa -C ansible-admin

#mkdir -pv /home/vagrant/tower-install 
# chown /home/ansible-admin/.ssh directory
chown -R ansible-admin:ansible-admin /home/ansible-admin/.ssh

# SELinux
setenforce 0

# Install gitlab runner
curl -L "https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.rpm.sh" | sudo bash

# Adjust sshd
sed -i 's/^#PasswordAuthentication yes/PasswordAuthentication yes/g' /etc/ssh/sshd_config
sed -i 's/^PasswordAuthentication no/#PasswordAuthentication no/g' /etc/ssh/sshd_config
sed -i 's/^#PubkeyAuthentication yes/PubkeyAuthentication yes/g' /etc/ssh/sshd_config
systemctl restart sshd

SCRIPT
##################################
## END OF Linux SCRIPT
##################################


##################################
##### START OF CentOS base server SCRIPT
##################################
$base_config = <<-SCRIPT

#yum install -y wget git python3 vim

#pip3 install --upgrade pip 

#pip3 install ansible --force-reinstall

useradd -m -d /home/ansible-admin -s /bin/bash ansible-admin

# SELinux
setenforce 0

SCRIPT
##################################
## END OF Linux SCRIPT
##################################

##################################
## START OF WINDOWS SCRIPT
##################################
#$powershell_runner_config = <<-SCRIPT
#
## disable the windows firewall
#netsh advfirewall set allprofiles state off
#
## install the powercli module
#Save-Module -Name VMware.PowerCLI -Path "c:\\Windows\\System32\\WindowsPowerShell\\v1.0\\Modules"
#
## disable hibernation
#powercfg /hibernate off
#
## setting power settings
#powercfg.exe -SETACTIVE 8c5e7fda-e8bf-4a96-9a85-a6e23a8c635c
#
## extend the eval license for windows (causes reboots if not)
#slmgr.vbs /rearm
#
#SCRIPT
##################################
## END OF Windows SCRIPT
##################################


##################################
# vagrant configuration
##################################
Vagrant.configure("2") do |config|
  config.vm.define "controller1" do |controller1|
    controller1.vm.box = "generic/rhel8"
    controller1.vm.hostname = "controller1"
    controller1.vm.provision "shell", inline: $controller_config
    controller1.vm.network "private_network" , ip: "192.168.59.10" 
    config.vm.provider "virtualbox" do |controllerm|
        controllerm.memory = 8192
        controllerm.cpus = 2
    end
  end

#  config.vm.define "controller2" do |controller2|
#    controller2.vm.box = "generic/rhel8"
#    controller2.vm.hostname = "controller2"
#    controller2.vm.provision "shell", inline: $controller_config
#    controller2.vm.network "private_network", ip: "192.168.59.12", auto_config: false
#    config.vm.provider "virtualbox" do |controller2m|
#        controller2m.memory = 8192
#        controller2m.cpus = 2
#    end
#  end

#  config.vm.define "database" do |database|
#    database.vm.box = "bento/centos-8.2"
#    database.vm.hostname = "database"
#    database.vm.provision "shell", inline: $base_config
#    database.vm.network "private_network" , ip: "192.168.59.100" 
#    config.vm.provider "virtualbox" do |databasem|
#        databasem.memory = 4096
#        databasem.cpus = 2
#    end
#  end

# Gitlab CE also installed on this node
  config.vm.define "execnode1" do |execnode1|
    execnode1.vm.box = "generic/rhel8"
    execnode1.vm.hostname = "execnode1"
    execnode1.vm.provision "shell", inline: $gitlab_config
    execnode1.vm.network "private_network" , ip: "192.168.56.11"
    config.vm.provider "virtualbox" do |execnodem1|
        execnodem1.memory = 4096
        execnodem1.cpus = 2
    end
  end

#  config.vm.define "execnode2" do |execnode2|
#    execnode2.vm.box = "bento/centos-8.2"
#    execnode2.vm.hostname = "execnode2"
#    execnode2.vm.provision "shell", inline: $controller_config
#    execnode2.vm.network "private_network" , ip: "192.168.56.12" 
#    execnode2.vm.synced_folder '.', '/vagrant', disabled: true
#    config.vm.provider "virtualbox" do |execnodem2|
#        execnodem2.memory = 4096
#        execnodem2.cpus = 2
#    end
#  end

#  config.vm.define "execnode3" do |execnode3|
#    execnode3.vm.box = "generic/rhel8"
#    execnode3.vm.hostname = "execnode3"
#    execnode3.vm.provision "shell", inline: $controller_config
#    execnode3.vm.network "private_network" , ip: "192.168.56.13" 
##    execnode3.vm.synced_folder '.', '/vagrant', disabled: true
#    config.vm.provider "virtualbox" do |execnodem3|
#        execnodem3.memory = 4096
#        execnodem3.cpus = 2
#    end
#  end

  config.vm.define "autohub" do |autohub|
    autohub.vm.box = "generic/rhel8"
    autohub.vm.hostname = "autohub"
    autohub.vm.provision "shell", inline: $base_config
    autohub.vm.network "private_network" , ip: "192.168.59.50" 
    config.vm.provider "virtualbox" do |autohubm|
        autohubm.memory = 8192
    end
  end

end

