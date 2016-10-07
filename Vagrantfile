# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "bento/ubuntu-16.04"

  # Disable automatic box update checking. The
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`.
  config.vm.box_check_update = false

  # Provision with a shell script
  config.vm.provision "shell", inline: <<-SHELL
    # enable source packages
    sudo sed -i -- 's/#deb-src/deb-src/g' /etc/apt/sources.list
    sudo sed -i -- 's/# deb-src/deb-src/g' /etc/apt/sources.list

    sudo apt update
    sudo apt install -y aptitude
    sudo aptitude install -y python-dev python3 python3-virtualenv
    sudo aptitude build-dep -y python-scipy

    # create the virtualenv
    python3 -m virtualenv --python python2.7 /home/vagrant/.virtualenvs/quantipy
    /home/vagrant/.virtualenvs/quantipy/bin/python -m pip install -r /vagrant/requirements.txt

    chown -R vagrant:vagrant /home/vagrant/.virtualenvs

    # workaround for repo is package
    ln -s /vagrant /home/vagrant/quantipy

    cat <<EOT >> /home/vagrant/.bashrc
export PYTHONPATH=/home/vagrant
. ~/.virtualenvs/quantipy/bin/activate
EOT

  SHELL

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
  end

end
