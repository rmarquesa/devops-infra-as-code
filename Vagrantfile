# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/focal64"
  config.vm.box_check_update = false

  vm_names=["k8s-master","k8s-node-1","k8s-node-2"]
  vm_names.each_with_index  do |vm, index|
    config.vm.define "#{vm}" do |node|
      node.vm.hostname = "#{vm}"
      node.vm.network "public_network", ip: "192.168.15.2#{index}", bridge: "Realtek PCIe GbE Family Controller"
      #node.vm.network "private_network", ip: "192.168.50.2#{index}"
      #node.vm.network "forwarded_port", guest: 80, host: 80
      node.vm.disk :disk, size: "20GB", primary: true, name: "disk-#{vm}"

      # Example for VirtualBox:
      node.vm.provider "virtualbox" do |vb|
        # Set VM name:
        vb.name = "#{vm}"

        # Display the VirtualBox GUI when booting the machine
        vb.gui = false

        # Customize the amount of memory on the VM:
        vb.memory = "4096"

        # Customiza the amount of cpu on the VM:
        vb.cpus = 2
      end

      # Enable provisioning with a shell script. Additional provisioners such as
      # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
      # documentation for more information about their specific syntax and use.
      node.vm.provision "shell", inline: "apt update && apt upgrade -y"
      node.vm.provision "shell", path: "scripts/enable_ssh_without_key.sh"
      node.vm.provision "shell", path: "scripts/install_tools.sh"
    end
  end
end