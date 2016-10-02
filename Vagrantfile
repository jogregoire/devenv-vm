# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "ubuntu/xenial64"

  if Vagrant.has_plugin?("vagrant-proxyconf")
    config.proxy.http     = ENV['http_proxy'] || ""
    config.proxy.https    = ENV['https_proxy'] || ""
    config.proxy.no_proxy = ENV['no_proxy'] || ""
  end

    config.vm.provider "virtualbox" do |vb|
    vb.memory = 1024
    vb.cpus = 1
    vb.customize ["modifyvm", :id, "--natnet1", "192.168/16"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    vb.customize ["modifyvm", :id, "--ioapic", "on"]
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.galaxy_role_file = "/vagrant/provisioning/requirements.yml"
    ansible.playbook = "/vagrant/provisioning/playbook.yml"
    ansible.inventory_path = "/vagrant/provisioning/inventory"
    ansible.limit = "all"
    ansible.sudo = true
  end

# In case you want to forward port to your host machine:
# config.vm.network "forwarded_port", guest: 3000, host: 3000

end
