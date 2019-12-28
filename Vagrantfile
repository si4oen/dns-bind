# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

Vagrant.configure(2) do |config|

	NodeCount = 2 #number of node to deploy

	#which host-port forward to box-port
	#config.vm.network "forwarded_port", guest: "#{GUEST_PORT}", host: "#{HOST_PORT}"

	#disable default /vagrant share
	config.vm.synced_folder ".", "/vagrant", disabled: true

	#ssh key-based authentication
	config.ssh.insert_key = false
    config.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/id_rsa']
    config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys"

    #If install VirtualBox Guest Additions set auto_update to "TRUE" or running command "vagrant vbguest --do install"
	#must install ==> vagrant plugin install vagrant-vbguest
    if Vagrant.has_plugin?("vagrant-vbguest") then
      config.vbguest.auto_update = false
      config.vbguest.no_remote = true
    end

	#run bootstrap script
	config.vm.provision "shell", path: "bootstrap.sh"

    #define vm properites
    (1..NodeCount).each do |i|
	config.vm.define "dns#{i}" do |node|
      node.vm.box = "box.centos7"
	  node.vm.hostname = "ns#{i}.testlab.local"
      node.vm.network "public_network", ip: "192.168.16.20#{i}"
	  node.vm.provider "virtualbox" do |vb|
        vb.name = "dns#{i}"
        vb.memory = 1024
        vb.cpus = 1
      end
	end
  end
end
