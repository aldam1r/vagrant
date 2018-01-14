# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
VAGRANTFILE_API_VERSION = "2"

# Get Settings
require 'yaml'
settings = YAML::load(File.read("settings.yaml"))

# Make settings local.
# Virtualbox files
SRC_FILE = settings['src_file']
VB_FILE = settings['vb_file']

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.box_check_update = settings['check_update'] # true
  config.vm.hostname = "andrastea.equinox"
  config.vm.network "public_network", ip: "192.168.178.113"
  config.vm.network "private_network", ip: "192.168.57.113"
  
  config.vm.define "ubuntu_docker"
  
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    # vb.gui = true
    vb.name = "andrastea"
    vb.cpus = 2
    vb.memory = "1024"
    
    if ARGV[0] == "up" && ! File.exist?(VB_FILE)

      vb.customize ['clonemedium', SRC_FILE, VB_FILE,
                    '--format', 'VDI']

      vb.customize ['storageattach', :id,
                    '--storagectl', 'SATAController',
                    '--port', 0,
                    '--device', 0,
                    '--type', 'hdd',
                    '--medium', VB_FILE]

      vb.customize ['closemedium', SRC_FILE,
                    '--delete']
                    
    end
   
  end
  
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    # apt-get install -y apache2
    mkdir -p /home/ubuntu/.ssh
    cat /vagrant/files/ssh/id_rsa.pub >> /home/ubuntu/.ssh/authorized_keys
  SHELL

end
