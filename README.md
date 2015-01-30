# devdep
Development and deployment environment for web applications.

DIAGRAM

## Overview

This documents how to link various tools and services together to develop web applications in a developer friendly and flexible way.

## Set up

### Developer workstation

The developer workstation should be a fairly powerful PC with plenty of RAM.  It will need to run a Virtual Machine (VM) comfortably as well as IDE's which may be fairly heavy on resources.


#### Standard tools to install

A document explaining how to set up a developer PC is available at:

https://github.com/freewayprojects/linux-pc-developer-setup-guide


#### Vagrant

The tool Vagrant should be installed - Vagrant config files and boxes will be made available in this repository.  Vagrant works well with VirtualBox which should be installed as well - although other VM programs could be used.

Apt-get install virtualbox build-essentials


For Ubuntu it is necessary to install a plugin to make sure the guest additions are at matching versions.  Details are at:

    http://kvz.io/blog/2013/01/16/vagrant-tip-keep-virtualbox-guest-additions-in-sync/
    https://github.com/dotless-de/vagrant-vbguest

Install the plugin as the normal user on the PC - not as root - as it is the user account which will be using the plugin.

Adding the boxes...

Boxes are added with

$ vagrant box add lucid32 http://files.vagrantup.com/lucid32.box


    
Set the VM's up in structure like ~/projects....../vagrant_machines/newmachine1/vm

and create a folder called ~/projects....../vagrant_machines/newmachine1/data

as this will be a folder where shared folders will be stored.  And chmod 777 this directory.

The server can be configured with a Vagrantfile which is in this repository.


$ vagrant init

Then set up the vagrant file to match the one required - which should be in the repository.

$ vagrant up


These are the tasks which need to be carried out on the server.  Later on they will be automated by either an install script or creating a new box file with the packages already set up.

    apt-get update and dist-upgrade all packages.

    # tasksel install lamp-server

    # a2enmod rewrite
    # apache2ctl restart
    
    apt-get install php5-intl php5-curl php5-gd php5-mcrypt php5-cli
    apt-get install php5-xdebug
    
And carry out some basic set up by editing the file:

    /etc/php5/conf.d/xdebug.ini
or

    /etc/php5/conf.d/20-xdebug.ini

and adding:

    xdebug.default_enable=1
    xdebug.remote_enable=1
    xdebug.remote_handler=dbgp
    xdebug.remote_host=192.168.33.1
    ;xdebug.remote_connect_back=1 ; Use remote_host or remote_connect_back. With a local VM remote_connect_back sho$
    xdebug.remote_port=9000
    xdebug.remote_autostart=0
    xdebug.remote_log=/tmp/php5-xdebug.log

Set up the vhosts as necessary.

The vhost file to use as a template is:

<VirtualHost *:80>

    ServerName surreyacuksite.acquialucid1
    ##    ServerAlias *.vhostname.serverfqdn
    ServerAdmin m06512@surrey.ac.uk
    
    DocumentRoot /vagrant_data/surreyacuksite/docroot
    
    <Directory /vagrant_data/surreyacuksite/docroot>
	AllowOverride all
	Order allow,deny
	Allow from all
    </Directory>
    
    # Possible values include: debug, info, notice, warn, error, crit,
    # alert, emerg.
    LogLevel warn

    # Make all scripts run as the specified user if Apache ITK is being used.
    <IfModule mpm_itk_module>
	AssignUserId username username
    </IfModule>

    # Limit PHP scripts to accessing only certain direectories.
    # NB - This can be worked around with shell_exec etc but is a level of protection.
    # php_admin_value open_basedir /vagrant_data/surreycuksite/docroot/:/usr/share/php/:/tmp

</VirtualHost>


On the work stations the hosts file will need to be set up like:

acquialucid1    192.168.33.10
test1.acquialucid1      192.168.33.10

    apt-get install mc git-core emacs
 
 
NB - NB - NB    apt-get install drush

Ap installs a (too) old version of drush - instead use commnds to at least get drush 5 installed

To work with Github etc we need to generate keys and upload them.

$ ssh-keygen

Choose DSA as this is slightly more seure than RSA.  No passphrase so that the commands can be scripted.

### Development scripts

Scripts should be available on the Vagrant VM to enable development.

#### Set up project script

One script should pull down code, load DB, set up vhost, change settings.

Download code from repo.


