# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
settings = YAML.load_file 'vagrant.yml'

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # Loop with node_number taking values from 1 to the configured cluster size
  (1..settings['cluster_size']).each do |node_number|

    # Define node_name by appending the node number to the configured vm_name
    node_name = settings['vm_name'] + "#{node_number}"

    # Define settings for each node
    config.vm.define node_name do |node|

      # The most common configuration options are documented and commented below.
      # For a complete reference, please see the online documentation at
      # https://docs.vagrantup.com.

      # Every Vagrant development environment requires a box. You can search for
      # boxes at https://vagrantcloud.com/search.
      node.vm.box = settings['vm_box']

      # Disable automatic box update checking. If you disable this, then
      # boxes will only be checked for updates when the user runs
      # `vagrant box outdated`. This is not recommended.
      # config.vm.box_check_update = false

      # Create a forwarded port mapping which allows access to a specific port
      # within the machine from a port on the host machine. In the example below,
      # accessing "localhost:8080" will access port 80 on the guest machine.
      # NOTE: This will enable public access to the opened port
      # config.vm.network "forwarded_port", guest: 80, host: 8080

      # Create a forwarded port mapping which allows access to a specific port
      # within the machine from a port on the host machine and only allow access
      # via 127.0.0.1 to disable public access
      # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

      # Create a private network, which allows host-only access to the machine
      # using a specific IP.
      # config.vm.network "private_network", ip: "192.168.33.10"
      node_ip = settings['vm_ip_prefix']+"."+"#{node_number+10}"
      node.vm.network "private_network", ip: node_ip
        # ,netmask: "255.255.255.0",
        # auto_config: true


      # Create a public network, which generally matched to bridged network.
      # Bridged networks make the machine appear as another physical device on
      # your network.
      # config.vm.network "public_network"

      # Share an additional folder to the guest VM. The first argument is
      # the path on the host to the actual folder. The second argument is
      # the path on the guest to mount the folder. And the optional third
      # argument is a set of non-required options.
      # config.vm.synced_folder "../data", "/vagrant_data"

      # Provider-specific configuration so you can fine-tune various
      # backing providers for Vagrant. These expose provider-specific options.
      # Example for VirtualBox:
      #
      node.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        vb.gui = settings['vm_gui']
      
        # Customize the amount of memory on the VM:
        vb.memory = settings['vm_memory']

        # Customize the name of the VM:
        vb.name = node_name
      end
      
      # View the documentation for the provider you are using for more
      # information on available options.

      # Enable provisioning with a shell script. Additional provisioners such as
      # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
      # documentation for more information about their specific syntax and use.
      # config.vm.provision "shell", inline: <<-SHELL
      #   apt-get update
      #   apt-get install -y apache2
      # SHELL
    end
  end
end
