# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
#  config.vm.box = "ubuntu/trusty64"
#
#  config.vm.define "compute", primary: true do |cpt|
#    cpt.vm.hostname = "controller"
#    cpt.vm.network "private_network", ip: "10.30.0.2", virtualbox__hostonly: "management"
#    cpt.vm.network "private_network", ip: "192.168.29.2", virtualbox__intnet: "external"
#    cpt.vm.network "private_network", ip: "10.99.0.2", virtualbox__intnet: "storage"
#    cpt.vm.network :forwarded_port, guest: 80, host: 8080
##    cpt.vm.network :forwarded_port, guest: 443, host: 44380
##    cpt.vm.network :forwarded_port, guest: 6080, host: 6080
#    config.vm.provider "virtualbox" do |vb|
#      vb.customize ["modifyvm", :id, "--cpus", "1"]
#      vb.customize ["modifyvm", :id, "--memory", "4096"]
#      vb.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
#      vb.customize ["modifyvm", :id, "--hwvirtex", "on"]
#    end
#  end

  config.vm.box = "ky1ebai/ceph-xenial"
  config.vm.define "ceph-aio" do |subconfig|
    subconfig.vm.hostname = "ceph-aio"
    subconfig.vm.network "private_network", ip: "10.30.0.3", virtualbox__hostonly: "management"
    subconfig.vm.network "private_network", ip: "192.168.29.2", virtualbox__intnet: "external"
    subconfig.vm.network "private_network", ip: "10.99.0.3", virtualbox__intnet: "storage"
    subconfig.vm.provider "virtualbox" do |vm|
      vm.name = "ceph-aio"
      vm.memory = 4096
      vm.cpus = 2
    end
    subconfig.vm.provider "virtualbox" do |vm|
      vm.customize ["modifyvm", :id, "--cpus", "4"]
      vm.customize ["modifyvm", :id, "--memory", "4096"]
      vm.customize ["modifyvm", :id, "--nicpromisc2", "allow-all"]
      vm.customize ["modifyvm", :id, "--hwvirtex", "on"]
    end        
    (1..3).each do |disk_id|
      subconfig.vm.provider "virtualbox" do |vm|
        vm.customize ["createhd", "--filename", "./tmp/disk#{disk_id}", "--size", "10240"]
        vm.customize [
          "storageattach", :id,
          "--storagectl", "SATA Controller",
          "--port", "#{disk_id}",
          "--type", "hdd",
          "--medium", "./tmp/disk#{disk_id}.vdi"]
      end
    end
  end
  config.vm.provision :shell, path: "./initial.sh"
end
