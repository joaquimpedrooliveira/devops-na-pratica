# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
VAGRANT_API_VERSION="2"

Vagrant.configure(VAGRANT_API_VERSION) do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "hashicorp/precise32"

  config.librarian_puppet.puppetfile_dir = "librarian"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  #Servidor de BD
  config.vm.define :db do |db_config|
    db_config.vm.hostname = "db"
    db_config.vm.network :private_network, :ip => "192.168.33.10"
    db_config.vm.provision "puppet" do |puppet|
        puppet.module_path = ["modules", "librarian/modules"]
        puppet.manifest_file = "db.pp"
    end
  end

  #Servidor de aplicação
  config.vm.define :web do |web_config|
    #Aumenta a memória padrão da VM somente para essa instância.
    web_config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end

    web_config.vm.hostname = "web"
    web_config.vm.network :private_network, :ip => "192.168.33.12"

    web_config.vm.provision "puppet" do |puppet|
        puppet.module_path = ["modules", "librarian/modules"]
        puppet.manifest_file = "web.pp"
    end
  end

  #Servidor de monitoramento
  config.vm.define :monitor do |monitor_config|
    monitor_config.vm.hostname = "monitor"
    monitor_config.vm.network :private_network, :ip => "192.168.33.14"
  end

  #Servidor de IC
  config.vm.define :ci do |build_config|
    build_config.vm.hostname = "build"
    build_config.vm.network :private_network, :ip => "192.168.33.16"

    #Aumenta a memória padrão da VM somente para essa instância.
    build_config.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm", :id, "--memory", "512"]
    end

    build_config.vm.provision "puppet" do |puppet|
        puppet.module_path = ["modules", "librarian/modules"]
        puppet.manifest_file = "ci.pp"
    end
  end


  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

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
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
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
