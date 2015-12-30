# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

VAGRANTFILE_API_VERSION = 2

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://atlas.hashicorp.com/search.
  config.vm.box = "box-cutter/centos67"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    # Customize the amount of memory on the VM:
    vb.memory = 1024 * 4
    vb.cpus = 4

    # Create a new disk for use by the btrfs filesystem, for Docker's use
    file_to_disk = './tmp/large_disk.vdi'
    unless File.exist?(file_to_disk)
      vb.customize ['createhd', '--filename', file_to_disk, '--size', 500 * 1024]
    end
    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]
  end

  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  config.vm.provision "shell", inline: <<-SHELL
    # Make sure the yum database is up-to-date.
    sudo yum -y update
    # Install the necessary repositories.
    sudo yum -y install epel-release
    sudo yum -y install redhat-lsb-core
    # Install the things necessary to get VirtualBox's host extensions to work.
    sudo yum -y install gcc
    sudo yum -y install dkms
    sudo yum -y install kernel-devel
    # Install developer tools
    sudo yum -y install emacs
    sudo yum -y install git
    # Install packages necessary for building some graphics tools (e.g. matplotlib).
    sudo yum -y install freetype-devel
    sudo yum -y install libpng-devel
    # Install packages to support btrfs
    sudo yum -y install btrfs-progs
    sudo mprobe btrfs
  SHELL

  config.vbguest.auto_update = true
  config.vbguest.no_remote = false
end
