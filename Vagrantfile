# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "genebean/centos-7-docker-ce"

  config.vm.network "forwarded_port", guest: 8080, host: 8880
  config.vm.network "forwarded_port", guest: 8500, host: 8500

  config.vm.provision "shell", inline: <<-SHELL
    set -ex
    curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
    chmod +x /usr/local/bin/docker-compose
    curl -L https://raw.githubusercontent.com/docker/compose/1.21.0/contrib/completion/bash/docker-compose -o /etc/bash_completion.d/docker-compose
  SHELL

  config.vm.provision "shell", inline: <<-SHELL
    set -ex
    export PATH=/usr/local/bin:$PATH
    docker-compose --version
    for image in `grep 'image:' /vagrant/docker-compose.yml|awk '{$1=$1;print}' |cut -d ' ' -f2`; do docker pull $image; done
    docker-compose -f /vagrant/docker-compose.yml up -d
    sleep 10
    curl --header 'Host:tester8000.lb.local' localhost
  SHELL
end
