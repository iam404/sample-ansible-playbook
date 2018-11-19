#@IgnoreInspection BashAddShebang
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

$script = <<-SCRIPT
  #!/bin/bash
  sudo apt-get update
  sudo apt-get -y install software-properties-common
  sudo apt-add-repository ppa:ansible/ansible -y
  sudo apt-get update
  sudo apt-get -y install ansible sshpass

  cat >/home/vagrant/.ssh/config <<EOL
   Host *
     StrictHostKeyChecking no


SCRIPT

config.vm.define "ansible-toolbox" do |trusty|
  trusty.vm.provider "virtualbox" do |v|
      v.memory = 512
  end
  trusty.vm.box = "ubuntu/trusty64"
  trusty.vm.hostname = "ansible-toolbox"
  trusty.vm.network "private_network", ip: "192.168.30.100", netmask: "255.255.255.0", auto_config: true, virtualbox__intnet: "net"
  trusty.ssh.forward_agent = true
  trusty.vm.provision "shell", inline: $script

end


 (1..3).each do |i|
  config.vm.define "test#{i}" do |host|
    host.vm.provider "virtualbox" do |v|
       v.memory = 1024
    end
    host.vm.box = "centos/6"
    host.vm.hostname = "Test#{i}"
    host.vm.network "forwarded_port", guest: 80, host: "908#{i}"
    host.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo service sshd restart", run: "always"
    host.vm.network "private_network", ip: "192.168.30.#{i}", netmask: "255.255.255.0", auto_config: true, virtualbox__intnet: "net"
    host.ssh.forward_agent = true
    host.ssh.insert_key = true
    #host.ssh.username = 'vagrant'
    #host.ssh.password = 'vagrant'


  end
 end
end




