# -*- mode: ruby -*-
# vi: set ft=ruby :

# vagrant rsync-auto


VAGRANTFILE_API_VERSION = "2"
PROJECT = "vm"
DESCRIPTION = "#{PROJECT} lab environment"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

if Vagrant.has_plugin?("vagrant-vbguest")
  config.vbguest.auto_update = false
end

############### vm ###############

    config.vm.define "vm" do |vm|
      vm.vm.box = "centos/8"
      vm.vm.hostname = "#{PROJECT}-vm"

      vm.vm.provider "virtualbox" do |vb|
        vb.memory = 1024
        vb.cpus = 1
        vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
        vb.customize ["modifyvm", :id, "--ioapic", "on"]
        vb.customize ["modifyvm", :id, "--description", "#{DESCRIPTION}"]
        vb.customize ["setextradata", :id, "GUI/HidLedsSync", 0]
      end
      vm.vm.network "forwarded_port", host_ip: "127.0.0.1", guest: 8000, host: 50080
      vm.vm.synced_folder ".", "/vagrant", type: "rsync", create: "true"
      vm.vm.provision "shell", run: "always", inline: <<-SHELL
        cd /vagrant
        yum install python3 -qy
        pip3 install -r requirements.txt
        mkdocs serve
      SHELL
    end

end

