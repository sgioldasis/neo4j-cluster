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

      # Every Vagrant development environment requires a box. You can search for
      # boxes at https://vagrantcloud.com/search.
      node.vm.box = settings['vm_box']

      # Determine node_ip based on the configured vm_ip_prefix 
      node_ip = settings['vm_ip_prefix']+"."+"#{node_number+10}"

      # Create a private network, which allows access to the machine using node_ip
      node.vm.network "private_network", ip: node_ip

      # Provider-specific configuration so you can fine-tune various
      # backing providers for Vagrant. These expose provider-specific options.
      # Below configuration targets VirtualBox:
      node.vm.provider "virtualbox" do |vb|
        # Display the VirtualBox GUI when booting the machine
        vb.gui = settings['vm_gui']
      
        # Customize the amount of memory on the VM:
        vb.memory = settings['vm_memory']

        # Customize the name of the VM:
        vb.name = node_name
      end
      
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
    end
  end
end
