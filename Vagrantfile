# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "master" do |master|
    master.vm.box = "centos/7"
    master.vm.hostname = "master"
    master.vm.box_check_update = false
    master.vm.network "private_network", ip: "192.168.1.10"
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "2048"
    end
  end

  config.vm.define "worker1" do |worker1|
    worker1.vm.box = "centos/7"
    worker1.vm.hostname = "worker1"
    worker1.vm.box_check_update = false
    worker1.vm.network "private_network", ip: "192.168.1.20"
    worker1.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
  end

  config.vm.define "worker2" do |worker2|
    worker2.vm.box = "centos/7"
    worker2.vm.hostname = "worker2"
    worker2.vm.box_check_update = false
    worker2.vm.network "private_network", ip: "192.168.1.30"
    worker2.vm.provider "virtualbox" do |vb|
      vb.memory = "1024"
    end
  end
end
