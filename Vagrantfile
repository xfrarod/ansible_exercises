# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.ssh.insert_key = false

  config.vm.synced_folder "./playbooks", "/vagrant_data"

  config.vm.define "vagrant1" do |vagrant1|
    vagrant1.vm.box = "ubuntu/xenial64"
    vagrant1.vm.network "public_network", bridge: "en0: Wi-Fi"

    vagrant1.vm.network "forwarded_port", guest: 3000, host: 3000
    vagrant1.vm.network "forwarded_port", guest: 80, host: 8080
    vagrant1.vm.network "forwarded_port", guest: 443, host: 8443
  end

  config.vm.define "vagrant2" do |vagrant2|
    vagrant2.vm.box = "ubuntu/xenial64"
    vagrant2.vm.network "public_network", bridge: "en0: Wi-Fi"

    # vagrant2.vm.network "forwarded_port", guest: 80, host: 8081
    # vagrant2.vm.network "forwarded_port", guest: 443, host: 8444
  end

  config.vm.define "vagrant3" do |vagrant3|
    vagrant3.vm.box = "ubuntu/xenial64"
    vagrant3.vm.network "public_network", bridge: "en0: Wi-Fi"

    # vagrant3.vm.network "forwarded_port", guest: 80, host: 8082
    # vagrant3.vm.network "forwarded_port", guest: 443, host: 8445
  end

end
