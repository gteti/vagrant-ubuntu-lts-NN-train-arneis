# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "nitindas/ubuntu-18"

  config.vbguest.auto_update = true

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"
  unique_hostport = 2225
  config.vm.network "forwarded_port", guest: 22, host: unique_hostport, host_ip: "127.0.0.1", id: "ssh"
  config.ssh.host = "localhost"
  config.ssh.port = unique_hostport

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
    vb.gui = true
  #

    # Customize the amount of cpus on the VM:
    vb.cpus = 2

    # Customize the amount of memory on the VM:
    vb.memory = "8192"
  end
  
  config.vm.disk :disk, size: "40GB", primary: true

  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  
  config.vm.provision "shell" do |s| #, inline: <<-SHELL
    ssh_pub_key = File.readlines("C:/Users/teti/.ssh/id_rsa_azure.pub").first.strip
    
    s.inline = <<-SHELL
    echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
    echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
    
    setxkbmap it
    sed -i 's/XKBLAYOUT=\"\w*"/XKBLAYOUT=\"it\"/g' /etc/default/keyboard
    
    echo -e "\n\n Starting main commands!"
    sudo adduser vagrant
    sudo adduser $USER vboxsf
    sudo apt-get update
    
    sudo apt install gnome-control-center
    
    sudo apt install git curl libcurl4
    #cd ~/Desktop
    mkdir Python
    cd Python
    mkdir tmp
    cd tmp
    echo -e "\n\nDownloading Anaconda installer. This may take sometime."
    curl -O https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
    echo -e "\n\n Download finished. Starting installation!"
    bash Anaconda3-2021.11-Linux-x86_64.sh -b -p /home/vagrant/anaconda3
    
    export PATH="/home/vagrant/anaconda3/bin:$PATH"
    echo 'export PATH="/home/vagrant/anaconda3/bin:$PATH"' >> /home/vagrant/.bashrc
    sudo chown -R $USER:$USER /home/vagrant/anaconda3

    conda init bash

    echo -e "\n\n Creating environment and installing major libraries\n"
    conda create --name trainenv python=3.6

    echo -e "\n\n Activating trainenv environment\n"
    conda activate trainenv

    conda install -c anaconda scipy
    conda install -c conda-forge pandas ipykernel
    pip install numpy==1.17.5 
    pip install tensorflow==1.15.0
    pip install jupyter 
    pip install tf_slim Cython contextlib2 pillow lxml matplotlib pycocotools gdown
    pip install google-api-python-client
    sudo apt-get update && sudo apt-get install -y -qq protobuf-compiler python-pil python-lxml python-tk

    echo -e "\n\n Installing OpenVINO\n"
    sudo apt-get install -y pciutils cpio

    echo -e "\n\nInstalling VSCODE\n"
    sudo snap install --classic code

    echo -e "\n\nFINISHED\n"
    #wget https://registrationcenter-download.intel.com/akdlm/irc_nas/17662/l_openvino_toolkit_p_2021.3.394.tgz
    #
    #tar xf l_openvino_toolkit_p_2021.3.394.tgz
    #cd l_openvino_toolkit_p_2021.3.394
    #echo -e "\n\ninstall_openvino_dependencies\n"
    #./install_openvino_dependencies.sh && \
    #    sed -i 's/decline/accept/g' silent.cfg && \
    #    ./install.sh --silent silent.cfg
    #
    #echo -e "\n\nsetupvars and demo_squeezenet_download_convert_run\n"    
    #source /opt/intel/openvino_2021.2.394/bin/setupvars.sh && \
    #    /opt/intel/openvino_2021.2.394/deployment_tools/demo/demo_squeezenet_download_convert_run.sh

    #   apt-get install -y apache2
  SHELL
  end
end
