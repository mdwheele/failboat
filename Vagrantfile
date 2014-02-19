# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

    config.vm.box = "centos65"
    config.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/centos-65-x64-virtualbox-puppet.box"
    config.vm.hostname = "local.dev"

    config.vm.network :private_network, ip: "192.168.33.10"
    config.vm.network :forwarded_port, guest: 80, host: 8080

    config.ssh.forward_agent = true

    config.vm.synced_folder "~/Sites", "/var/www/html"

    config.vm.provider :virtualbox do |vb|
        vb.customize [
            "modifyvm", :id,
            "--memory", "1024"
        ]
    end

    # Copy host .gitconfig to Vagrant home directory.
    config.vm.provision(:shell, :inline => "/bin/cat <<'EOT' >/home/vagrant/.gitconfig \n#{File.read("#{Dir.home}/.gitconfig")}\nEOT") if File.exist?("#{Dir.home}/.gitconfig")
    config.vm.provision(:shell, :inline => "echo 'Your Failboat is leaking:\n#{Dir.home}/.gitconfig does not exist!!\nYou may need to configure git!!!'") unless File.exist?("#{Dir.home}/.gitconfig")

    config.vm.provision :puppet do |puppet|
        puppet.module_path = "puppet/modules"
        puppet.manifests_path = "puppet/manifests"
        puppet.manifest_file  = "site.pp"
        puppet.options = "--hiera_config /vagrant/hiera.yaml"
    end
end
