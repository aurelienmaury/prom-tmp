# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vbguest.auto_update = false


  config.vm.define "slave_1" do |slave_1|
    slave_1.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
      vb.name = "slave_1"
    end
    slave_1.vm.network "private_network", ip: "192.168.27.10"

    slave_1.vm.provision "ansible" do |ansible|
      ansible.groups = {
          "slaves" => ["slave_1"],
          "all_groups:children" => ["slaves"]
      }
      ansible.playbook = "prometheus-all.yml"
    end
  end


  config.vm.define "slave_2" do |slave_2|
    slave_2.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
      vb.name = "slave_2"
    end
    slave_2.vm.network "private_network", ip: "192.168.27.11"
    slave_2.vm.provision "ansible" do |ansible|
      ansible.groups = {
          "slaves" => ["slave_2"],
          "all_groups:children" => ["slaves"]
      }
      ansible.playbook = "prometheus-all.yml"
    end
  end


  config.vm.define "master" do |master|
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
      vb.name = "master"
    end

    master.vm.network "private_network", ip: "192.168.27.1"
    master.vm.network "forwarded_port", guest: 9090, host: 9090
    master.vm.network "forwarded_port", guest: 9100, host: 9100
    master.vm.network "forwarded_port", guest: 3000, host: 3000

    master.vm.provision "ansible" do |ansible|
      ansible.groups = {
          "master" => ["master"],
          "slaves" => ["slave_1", "slave_2"],
          "all_groups:children" => ["master", "slaves"]
      }
      ansible.playbook = "prometheus-all.yml"
      ansible.limit = "all"
    end
  end


end
