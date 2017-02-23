# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "centos/7"

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL

  config.vm.define "prom_master" do |web|
    config.vm.provider "virtualbox" do |vb|
      vb.memory = "512"
      vb.name = "prom_master"
    end
  end

  config.vm.provision "ansible" do |ansible|
    ansible.groups = {
      "prom" => ["prom_master"],
      "all_groups:children" => ["prom"]
    }
    ansible.playbook = "prometheus.yml"
  end

end
