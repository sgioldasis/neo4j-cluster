# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'
settings = YAML.load_file 'vagrant.yml'

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # Loop to determine cluster-wide lists
  # neo4j_initial_hosts = ""
  # (1..settings['cluster_size']).each do |node_number|
  #   node_ip = settings['vm_ip_prefix']+"."+"#{node_number+10}"
  #   host_entry = node_ip + ":" + settings['neo4j_host_coordination_port'].to_s
  #   if neo4j_initial_hosts == "" then
  #     neo4j_initial_hosts = host_entry
  #   else
  #     neo4j_initial_hosts = neo4j_initial_hosts # + "," + host_entry
  #   end
  # end
  # print "Determined: neo4j_initial_hosts = " + neo4j_initial_hosts + "\n"

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

      # Determine node_ip based on the configured vm_ip_prefix 
      node_ip = settings['vm_ip_prefix']+"."+"#{node_number+10}"

      # Create a private network, which allows access to the machine using node_ip
      node.vm.network "private_network", ip: node_ip


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

      # Ansible provisioning

      # Disable the new default behavior introduced in Vagrant 1.7, to
      # ensure that all Vagrant machines will use the same SSH key pair.
      # See https://github.com/mitchellh/vagrant/issues/5005
      node.ssh.insert_key = false

      # Determine neo4j_initial_hosts 
      initial_node_ip = settings['vm_ip_prefix']+"."+"11"
      host_coordination_port = settings['neo4j_host_coordination_port'].to_s
      neo4j_initial_hosts = initial_node_ip + ":" + host_coordination_port

      # Call Ansible also passing it values needed for configuration
      node.vm.provision "ansible" do |ansible|
        ansible.verbose = "v"
        ansible.playbook = "playbook.yml"
        ansible.extra_vars = {
          node_ip_address: node_ip,
          neo4j_server_id: node_number,
          neo4j_initial_hosts: neo4j_initial_hosts
      }
      end

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
