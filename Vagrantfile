# -*- mode: ruby -*-
# vi: set ft=ruby :
$rmq_script = <<SCRIPT
  echo I am provisioning...
  sudo yum -y update

  sudo yum -y install wget
  wget https://www.rabbitmq.com/releases/erlang/erlang-18.1-1.el7.centos.x86_64.rpm
  sudo yum -y install erlang-18.1-1.el7.centos.x86_64.rpm

  wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.5.7/rabbitmq-server-3.5.7-1.noarch.rpm
  sudo rpm --import https://www.rabbitmq.com/rabbitmq-signing-key-public.asc
  sudo yum -y install rabbitmq-server-3.5.7-1.noarch.rpm

  sudo chkconfig rabbitmq-server on
  sudo /sbin/service rabbitmq-server start

  sudo rabbitmq-plugins enable rabbitmq_management
  sudo rabbitmq-plugins enable rabbitmq_federation
  sudo rabbitmq-plugins enable rabbitmq_federation_management

  sudo rabbitmqctl add_user admin admin
  sudo rabbitmqctl set_user_tags admin administrator
  sudo rabbitmqctl set_permissions -p / admin \".*\" \".*\" \".*\"

SCRIPT

$srv_script = <<SCRIPT
  sudo docker pull irybakov/async-api
  sudo docker run -it -d \
      -p 8082:8082 \
      -e AMQP_HOST=192.168.33.10 \
      -e AMQP_PORT=5672 \
      -e AMQP_USER=admin \
      -e AMQP_PASSWORD=admin \
      irybakov/async-api

  sudo docker pull irybakov/async-stub
  sudo docker run -it -d \
      -e AMQP_HOST=192.168.33.10 \
      -e AMQP_PORT=5672 \
      -e AMQP_USER=admin \
      -e AMQP_PASSWORD=admin \
      irybakov/async-stub
SCRIPT

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search  
  config.vm.define "rmqc" do |rmc|
    rmc.vm.box = "centos/7"
    rmc.vm.network "private_network", ip:"192.168.33.10"
    rmc.vm.network "forwarded_port", guest: 15672, host: 15672
    rmc.vm.provision "shell", inline: $rmq_script

    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "2048"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
      #rmc.vm.provision "shell" do |s|
      #s.inline = $script
      #s.inline = "sudo yum -y update"
      #s.inline = "sudo yum -y install wget"
      # erlang
      #s.inline = "wget https://www.rabbitmq.com/releases/erlang/erlang-18.1-1.el7.centos.x86_64.rpm"
      #s.inline = "sudo yum -y install erlang-18.1-1.el7.centos.x86_64.rpm"
      # rabbitmq
      #s.inline = "wget https://www.rabbitmq.com/releases/rabbitmq-server/v3.5.7/rabbitmq-server-3.5.7-1.noarch.rpm"
      #s.inline = "sudo rpm --import https://www.rabbitmq.com/rabbitmq-signing-key-public.asc"
      #s.inline = "sudo yum -y install rabbitmq-server-3.5.7-1.noarch.rpm"
      # start on boot
      #s.inline = "sudo chkconfig rabbitmq-server on"
      #s.inline = "sudo /sbin/service rabbitmq-server start"
      # install plugins
      #s.inline = "sudo rabbitmq-plugins enable rabbitmq_management"
      #s.inline = "sudo rabbitmq-plugins enable rabbitmq_federation"
      #s.inline = "sudo rabbitmq-plugins enable rabbitmq_federation_management"
      # add user
      #s.inline = "sudo rabbitmqctl add_user admin admin"
      #s.inline = "sudo rabbitmqctl set_user_tags admin administrator"
      #s.inline = "sudo rabbitmqctl set_permissions -p / admin \".*\" \".*\" \".*\""      
      #end
  end


  config.vm.define "services" do |rm1|
    rm1.vm.box = "williamyeh/centos7-docker"
    rm1.vm.network "private_network", ip:"192.168.34.10"
    rm1.vm.network "forwarded_port", guest: 15672, host: 15673
    config.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", "1024"]
        vb.customize ["modifyvm", :id, "--cpus", "2"]
    end
    rm1.vm.provision "shell", inline: $srv_script
      
  end


  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 15672, host: 15672

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  #config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Display the VirtualBox GUI when booting the machine
    vb.gui = false
  
     # Customize the amount of memory on the VM:
     vb.memory = "1024"
   end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Define a Vagrant Push strategy for pushing to Atlas. Other push strategies
  # such as FTP and Heroku are also available. See the documentation at
  # https://docs.vagrantup.com/v2/push/atlas.html for more information.
  # config.push.define "atlas" do |push|
  #   push.app = "YOUR_ATLAS_USERNAME/YOUR_APPLICATION_NAME"
  # end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   sudo apt-get update
  #   sudo apt-get install -y apache2
  # SHELL
end
