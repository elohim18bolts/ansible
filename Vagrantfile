# -*- mode: ruby -*-
# vi: set ft=ruby :
#
#
require 'yaml'
PROVISIONER = "parallels"

def test_k3s_role(config,inventoryPath)
  yamlFile= YAML.load_file(inventoryPath)
  master_node = (yamlFile["k3s_masters"]["hosts"]).keys
  #master_node = []
  nodes = (yamlFile['k3s_nodes']['hosts']).keys
  nodes = master_node.concat(nodes)

  raw_ssh_key_args = []

  nodes.each_with_index do |node,index|
    config.vm.define "node-#{index}" do |n|
      n.vm.network "private_network",ip:"#{node}"
      n.vm.hostname = "node-#{index}"
      raw_ssh_key_args << "-o IdentityFile=.vagrant/machines/node-#{index}/#{PROVISIONER}/private_key"
      if index == (nodes.length - 1)
        sleep(2)
        n.vm.provision :ansible do |ansible|
          ansible.verbose = "v"
          ansible.limit = "all"
          ansible.inventory_path = "vagrant/inventory.yaml"
          ansible.playbook = "vagrant/vagrant_play.yaml"
          ansible.raw_ssh_args = raw_ssh_key_args
        end
      end
    end
  end
end

def test_common_role(config, inventoryPath)
  yamlFile = YAML.load_file(inventoryPath)
  host = (yamlFile['test_node']['hosts']).keys[0]
  config.vm.define "test_node" do |node|
    node.vm.network "private_network",ip:"#{host}"
    node.vm.hostname = "test-node"
    node.vm.provision :ansible do |ansible|
      ansible.verbose = "v"
      ansible.limit = "all"
      ansible.inventory_path = "vagrant/inv_single_node.yaml"
      ansible.playbook = "vagrant/vagrant_play.yaml"
      ansible.raw_arguments = [
        "--ask-vault-pass"
      ]
    end
  end
end

#
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "bento/ubuntu-22.04-arm64"

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
  #
   config.vm.provider :PROVISIONER do |prl|
     # Display the VirtualBox GUI when booting the machine
     #vb.gui = true
     # Customize the amount of memory on the VM:
     prl.memory = "1024"
   end

   #test_k3s_role(config,"./vagrant/inventory.yaml")
   test_common_role(config,"vagrant/inv_single_node.yaml")


end
