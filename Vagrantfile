# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 1.6.3"

$script = <<SCRIPT

  # Install docker-compose (FIG)
  curl -L https://github.com/docker/compose/releases/download/1.2.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose && chmod +x /usr/local/bin/docker-compose;

SCRIPT

# All Vagrant configuration is done below. The VAGRANTFILE_API_VERSION
# in Vagrant.configure configures the configuration version (we support
# older styles for backwards compatibility). Please don't change it unless
# you know what you're doing.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Setup CentOS 6.6 box
  config.vm.box = "chef/centos-6.6"

  # Set hostname to host machine's hostname
  config.vm.hostname = "#{`hostname`[0..-2]}-vagrant"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:9200" will access port 9200 on the guest machine.
  config.vm.network "forwarded_port", guest: 5601, host: 5601
  config.vm.network "forwarded_port", guest: 9200, host: 9200
  config.vm.network "forwarded_port", guest: 5000, host: 5000

  # This is a hack around the networking slowness in the VM.
  config.vm.provider :virtualbox do |vb|
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end

  # Enable Docker provisioning
  config.vm.provision "docker"

  config.vm.provision "shell", inline: $script

end