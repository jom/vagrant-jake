Vagrant.configure("2") do |config|
  config.vm.box = "debian-wheezy72-x64-vbox43"
  config.vm.box_url = "https://puphpet.s3.amazonaws.com/debian-wheezy72-x64-vbox43.box"

  config.vm.network "private_network", ip: "192.168.50.100"
  config.vm.network "forwarded_port", guest: 9000, host: 9000
  config.vm.synced_folder "/Users/jacob/Projects", "/vagrant-sync", id: "vagrant-root", type: "nfs"
  config.bindfs.bind_folder "/vagrant-sync", "/var/www", :owner => "www-data", :group => "www-data"

  config.vm.usable_port_range = (2200..2250)
  config.vm.provider :virtualbox do |virtualbox|
    virtualbox.customize ["modifyvm", :id, "--name", "dev"]
    virtualbox.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    virtualbox.customize ["modifyvm", :id, "--memory", "1024"]
    virtualbox.customize ["modifyvm", :id, "--cpus", "2"]
    virtualbox.customize ["setextradata", :id, "--VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
  end

  config.vm.provision :shell, :path => "shell/initial-setup.sh"
  config.vm.provision :shell, :path => "shell/update-puppet.sh"
  config.vm.provision :shell, :path => "shell/librarian-puppet-vagrant.sh"
  config.vm.provision :puppet do |puppet|
    puppet.facter = {
      "ssh_username" => "vagrant"
    }

    puppet.manifests_path = "puppet/manifests"
    puppet.options = ["--verbose", "--hiera_config /vagrant/hiera.yaml", "--parser future"]
  end

  # config.dns.tld = "dev"
  config.vm.hostname = "jake"

  config.ssh.username = "vagrant"

  config.ssh.shell = "bash -l"

  config.ssh.keep_alive = true
  config.ssh.forward_agent = false
  config.ssh.forward_x11 = false
  config.vagrant.host = :detect
  
  VagrantDNS::Config.auto_run = false
end

