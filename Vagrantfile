# -*- mode: ruby -*-
# vi: set ft=ruby :

# 
# This is a Vagrant configuration for a virtual machine designed
# for local development by a team of web developers.
#

require 'yaml'

VAGRANTFILE_API_VERSION = "2"
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

vagrant_dir = File.expand_path(File.dirname(__FILE__))



# Include local configurations for paths synced with the webroot
# Copy local.yml.example to local.yml if you have not done so
local_exists = false
if File.exists?(File.join(vagrant_dir,'local.yml')) then
    local_exists = true
    local_config = YAML.load_file(File.join(vagrant_dir, 'local.yml'))
end



  config.vm.define "web" do |web|

    # This is a CentOS 6.5 base image.
    web.vm.box = "centos65"
    web.vm.box_url = "https://github.com/2creatives/vagrant-centos/releases/download/v6.5.3/centos65-x86_64-20140116.box"

    # Automatically check for a new box image on 'vagrant up'
    web.vm.box_check_update = true

    # Forward VM webserver to local port 8080
    # Can access VM web at localhost:8081 in browser
    web.vm.network "forwarded_port", guest: 80, host: local_config['forwarded_http_port']
    # Forward VM MySQL to local port 3306
    web.vm.network "forwarded_port", guest: 3306, host: local_config['forwarded_mysql_port']

    web.vm.network "private_network", ip: "192.168.33.10"

    # Include local configurations for paths synced with the webroot
    # Copy local.yml.example to local.yml if you have not done so
    if local_exists then
      local_config['synced'].each do | synced |
        web.vm.synced_folder synced['local_path'], synced['server_path'], :mount_options => [ "dmode=775", "fmode=774" ]
      end
    end
  end # end 'web' config

  config.vm.provider "virtualbox" do |vb|
      #vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/vagrant-root", "1"]
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
  end
  #

  config.vm.provision "ansible" do |ansible|
    if local_exists then
        ansible.extra_vars = {
            env: local_config['env'], # "development" or "production"
            database_users: local_config['database_users'],
            databases: local_config['databases'],
        }
    end
    ansible.playbook = "provisioning/site.yml"
    ansible.tags="mysql"
  end
end
