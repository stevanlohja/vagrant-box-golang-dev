# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.box_version = "20160119.0.0"

  # Keep insecure keypair
  config.ssh.insert_key = false

  config.vm.provision "shell", privileged: true, inline: <<-SHELL

    sudo apt-get update

    # Install docker-compose
    curl -L https://github.com/docker/compose/releases/download/1.6.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose

    # Install go
    curl -O https://storage.googleapis.com/golang/go1.6.linux-amd64.tar.gz
    tar -C /usr/local -xzf go1.6.linux-amd64.tar.gz
    echo export PATH=\\\$PATH:/usr/local/go/bin >> /etc/profile

    # Install git
    apt-get install -y git

    # Install inotify
    apt-get install -y inotify-tools
  SHELL

  config.vm.provision "shell", privileged: false, inline: <<-SHELL

    # Setup go workspace, see https://golang.org/doc/code.html
    grep '^export GOPATH' ~/.bashrc || echo export GOPATH=~/go >> ~/.bashrc
    grep '^export PATH' ~/.bashrc || echo export PATH=\\\$PATH:~/go/bin >> ~/.bashrc

    # Install godep, see https://github.com/tools/godep
    GOPATH=~/go /usr/local/go/bin/go get github.com/tools/godep
  SHELL

  # Install latest docker
  config.vm.provision :docker do |d|
  end
end
