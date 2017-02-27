# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"
  config.vbguest.auto_update = false

  config.vm.define "master" do |master|
    master.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
      vb.name = "master"
    end
    master.vm.network "forwarded_port", guest: 9090, host: 9090
    master.vm.network "forwarded_port", guest: 9100, host: 9100
    master.vm.network "forwarded_port", guest: 3000, host: 3000

    master.vm.provision "ansible" do |ansible|
      ansible.groups = {
          "prom" => ["prom_master"],
          "all_groups:children" => ["prom"]
      }
      ansible.playbook = "prometheus-master.yml"
#    ansible.raw_arguments = ["-vvv"]
    end
  end

  config.vm.define "slave" do |slave|
    slave.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
      vb.name = "slave"
    end

    slave.vm.provision "ansible" do |ansible|
      ansible.groups = {
          "prom" => ["slave"],
          "all_groups:children" => ["prom"]
      }
      ansible.playbook = "prometheus.yml"
#    ansible.raw_arguments = ["-vvv"]
    end
  end

  config.vm.define "slave_2" do |slave_2|
    slave_2.vm.provider "virtualbox" do |vb|
      vb.memory = "256"
      vb.name = "slave_2"
    end

    slave_2.vm.provision "ansible" do |ansible|
      ansible.groups = {
          "prom" => ["slave_2"],
          "all_groups:children" => ["prom"]
      }
      ansible.playbook = "prometheus-slave.yml"
#    ansible.raw_arguments = ["-vvv"]
    end
  end
end
