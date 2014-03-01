# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "centos65"
    config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/centos-65-x64-virtualbox-puppet.box"
    config.vm.hostname = "local.dev"

    config.vm.network :private_network, ip: "192.168.33.10"
    config.vm.network :forwarded_port, guest: 80, host: 8080, host_ip: "127.0.0.1"

    config.vm.synced_folder "~/Sites", "/var/www/html"
    config.vm.synced_folder "./files/rpm", "/repository"

    config.ssh.forward_agent = true

    config.vm.provider :virtualbox do |vm|
        vm.name = "local_dev_vm"
        vm.memory = 1024
    end

    config.vm.provision "shell" do |s|
        s.path = "shell/initial-setup.sh"
        s.args = "/vagrant"
    end

    config.vm.provision :puppet do |puppet|
        puppet.module_path = "puppet/modules"
        puppet.manifests_path = "puppet/manifests"
        puppet.manifest_file  = "site.pp"
        puppet.options = "--hiera_config /vagrant/hiera.yaml"
    end

    config.vm.provision :shell, :path => "shell/update.sh"
end
