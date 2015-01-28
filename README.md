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

as this will be a folder where shared folders will be stored.  And chmod 777 this directory.

Then in the Vagrantfile add in the share:

    config.vm.synced_folder "../site_files", "/home/sites"

Default items to set up:

apt-get update and dist-upgrade all packages.

tasksel install lamp-server

   # a2enmod rewrite
    # apache2ctl restart
    
    apt-get install php5-intl php5-curl php5-gd php5-mcrypt php5-cli
    
    apt-get install php5-xdebug
    
    And carry out some basic set up by editing the file:

    /etc/php5/conf.d/xdebug.ini
or

    /etc/php5/conf.d/20-xdebug.ini
and adding

    xdebug.remote_enable=1
    xdebug.remote_handler=dbgp
    xdebug.remote_mode=req
    xdebug.remote_connect_back=1
    xdebug.remote_port=9000
    xdebug.show_local_vars=1










##Add networking to a bootstrap.sh file and reference it as per vagrantup.com

Install drush.

enable mod rewrite

add php5-xdebug




