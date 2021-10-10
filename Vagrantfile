# -*- mode: ruby -*-
# vi: set ft=ruby :
# This file can be used to setup Linux local virtual machines.
# Pre-requist to use this file is to:
# install virtual box on your windows machine.
# install vagrant from windows cmd (to check if installed, use vagrant --version)
# To run the vagrant file below, run command:  vagrant up
# This will create the 2 Linux VMs defined below.
# To check the vagrant hosts created, type: vagrant status
# To access the created hosts, type:
# vagrant shh host1 --> this will take you to host1 which is centos7. Do "yum" in the host to validate that.
# vagrant shh host2 --> this will take you host2 which is ubuntu. Type "apt-get" in the ubuntu to validate that.
# Another way to access these hosts is to use the username and ipaddress:
# ssh vagrant@192.168.205.10
# the first time you run the above command, you get permission denied. So you need to do few stuff first:
# Do: vagrant ssh host1
# When in the machine do: sudo systemctl restart sshd
# Then exit the VM and try to SSH again" ssh vagrant@192.168.205.10
# the password is vagrant
# Do same thing for host2: ssh vagrant@192.168.205.11
# To check the hosts status, do: vagrant status --> this will show both as running
# To stop the linux hosts, do: vagrant halt
# vagrant status --> this will show both hosts as poweroff
# To remove the hosts, do: vagrant destroy
# vagrant status --> show both hsts as "not created" because they have been deleted.

# NOTE:
# If you want to use vagrant with Hyper-V on Windows system, search on google for "vagrant windows hyper-v" and there are some documentation to reference: https://computingforgeeks.com/enable-hyper-v-and-install-vagrant-in-windows/


Vagrant.require_version ">= 1.6.0"

boxes = [

    {
        :name => "host1",
        :eth1 => "192.168.205.10",
        :mem => "1024",
        :cpu => "1",
        :image => "centos/7"
    },
    {
        :name => "host2",
        :eth1 => "192.168.205.11",
        :mem => "1024",
        :cpu => "1",
        :image => "generic/ubuntu1804"
    }
]

Vagrant.configure(2) do |config|
  boxes.each do |opts|
    config.vm.define opts[:name] do |config|
      config.vm.box = opts[:image]
      config.vm.hostname = opts[:name]
      config.vm.provider "vmware_fusion" do |v|
        v.vmx["memsize"] = opts[:mem]
        v.vmx["numvcpus"] = opts[:cpu]
      end
      config.vm.provider "virtualbox" do |v|
        v.customize ["modifyvm", :id, "--memory", opts[:mem]]
        v.customize ["modifyvm", :id, "--cpus", opts[:cpu]]
      end
      config.vm.network :private_network, ip: opts[:eth1]
    end
  end
  config.vm.provision "shell", inline:"sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config"
end
