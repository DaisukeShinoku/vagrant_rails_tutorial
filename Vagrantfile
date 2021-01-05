# -*- mode: ruby -*-
# vi: set ft=ruby :
# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  GUEST_RUBY_VERSION = '2.6.6'
  GUEST_RAILS_VERSION = '5.1.6'
  config.vm.box = "bento/ubuntu-18.04"
  config.vm.box_check_update = false
  config.vm.network "forwarded_port", guest: 3000, host: 3000
  config.vm.network "private_network", ip: "192.168.33.10"
  config.vm.synced_folder "./", "/home/vagrant/work"
  config.ssh.forward_agent = true
  config.vm.provider "virtualbox" do |vb|
      vb.gui = false
  end
  config.vm.provision "shell", inline: <<-SHELL
      echo '### installing tools ###'
      sudo timedatectl set-timezone Asia/Tokyo
      sudo apt update -y
      sudo apt upgrade -y
      sudo apt install build-essential -y
      sudo apt install -y libssl-dev libreadline-dev zlib1g-dev
      sudo apt install -y imagemagick
  SHELL
  config.vm.provision "shell", privileged: false, inline: <<-SHELL
      echo '### installing Ruby ###'
      git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
      echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bash_profile
      echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
      source ~/.bash_profile
      git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
      rbenv install #{GUEST_RUBY_VERSION}
      rbenv global #{GUEST_RUBY_VERSION}
      echo '### installing Rails ###'
      gem install rails -v #{GUEST_RAILS_VERSION}
      echo '### installing SQLITE3 ###'
      sudo apt install libsqlite3-dev
      sudo apt install sqlite3
      echo '### installing NodeJS ###'
      sudo apt install -y nodejs npm
      sudo npm install n -g
      sudo n lts
      sudo apt purge -y nodejs npm
      sudo apt -y autoremove
      echo '### increasing inotify ###'
      sudo sh -c "echo fs.inotify.max_user_watches=524288 >> /etc/sysctl.conf"
      sudo sysctl -p
      echo ' -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-'
      echo 'You are now on Rails!'
      echo ' -+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-'
  SHELL
end