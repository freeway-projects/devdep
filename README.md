# devdep
Development and deployment environment for web applications.

DIAGRAM

## Set up

### Developer workstation

#### Standard tools to install

#### VM

#### Vagrant

The tool Vagrant should be installed - Vagrant config files and boxes will be made available in this repository.

For Ubuntu it is necessary to install a plugin to make sure the guest additions are at matching versions.  Details are at:

    http://kvz.io/blog/2013/01/16/vagrant-tip-keep-virtualbox-guest-additions-in-sync/
    https://github.com/dotless-de/vagrant-vbguest
    
Set the VM's up in structure like ~/projects....../vagrant_machines/newmachine1/vm

and create a folder called ~/projects....../vagrant_machines/newmachine1/site_files

as this will be a folder where shared folders will be stored.

and on the VM create a directory called /home/sites as root - 755 root:root

Then in the Vagrantfile add in the share:

    config.vm.synced_folder "../site_files", "/home/sites"

Default items to set up:

apt-get update and dist-upgrade all packages.

mkdir ./sitefile on the host ins


tasksel lamp-server

##Add networking to a bootstrap.sh file and reference it as per vagrantup.com

Install drush.

enable mod rewrite

add php5-xdebug




