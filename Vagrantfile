# -*- mode: ruby -*-
# vi: set ft=ruby :
#
require 'yaml'
file = YAML.load_file("./vagrant/inventory.yaml")
master_node = file['vagrant_masters']['hosts']
nodes = file['vagrant_nodes']['hosts']
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

  config.vm.define "master" do |master|
    # Create a private network, which allows host-only access to the machine
    # using a specific IP.
    master.vm.network "private_network", ip: "#{master_node[0]}"
    master.vm.hostname = "vagrant-master"

    #
     master.vm.provider "parallels" do |prl|
       # Display the VirtualBox GUI when booting the machine
       #vb.gui = true
       # Customize the amount of memory on the VM:
       prl.memory = "1024"
     end
   # config.vm.provision "ansible" do |ansible|
   #   ansible.verbose = "v"
   #   ansible.limit = "all"
   #   ansible.inventory_path = "vagrant/inventory.yaml"
   #   ansible.playbook = "vagrant/vagrant_play.yaml"
   # end
  end
  nodes.each_with_index do |node,index|
    config.vm.define "#{node}" do |n|
      n.vm.network "private_network",ip:"#{node}"
      n.vm.hostname = "node-#{index}"
      n.vm.provider "parallels" do |prl|
        prl.memory = "1024"
      end
    end
  end

end
