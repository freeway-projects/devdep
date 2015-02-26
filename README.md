# Project name: Development and deployment environment for web applications.
# Project title: devdep



![alt text](https://docs.google.com/drawings/d/1e-bYXsE9qtk0sOts_jJxmsNMz1Mhxa_Sck-aXAY195M/pub?w=960&amp;h=720 "Logo Title Text 1")


![alt text](https://docs.google.com/drawings/d/1T_M633W0tE06bcuB1LsS3eH14eaeEcBBffnV3-7pdVM/pub?w=960&h=720 "Logo Title Text 2")

## Overview

This documents how to link various tools and services together to develop web applications in a developer friendly and flexible way.

== Set up ==

=== Developer workstation ===

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

Increase the memory used by MySql - use my-huge.cnf or similar.

If using my-huge the line:

    user = mysql 

needs to be added to the file in the mysqld section - as it is in the original my.cnf.


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


    root@server1:~# wget --quiet -O - http://ftp.drupal.org/files/projects/drush-7.x-5.8.tar.gz  | tar -zxf - -C /usr/local/share
    root@server1:~# ln -s /usr/local/share/drush/drush /usr/local/bin/drush
    root@server1:~# drush
    root@server1:~# drush dl drush --destination=/usr/local/share


To work with Github etc we need to generate keys and upload them.  Make sure you are the user 'vagrant'.

$ ssh-keygen

Choose DSA as this is slightly more seure than RSA.  No passphrase so that the commands can be scripted.

Then upload the public key to Github.


### Development scripts

Scripts should be available on the Vagrant VM to enable development.

#### Set up project script

One script should pull down code, load DB, set up vhost, change settings.

Download code from repo.

    git clone

Create DB and user.

    mysqladmin -u root -p create surreyacuksite
    mysql -u root -p --execute="GRANT ALL ON surreyacuksite.* TO 'surreyacuksite'@'localhost' IDENTIFIED BY 'uos3';" surreyacuksite


Download DB and load it into the DB.

Create new sites/default/local.settings.php with only the DB settings in it.

    <?php

    $databases['default']['default'] = array(
    'driver' => 'mysql',
    'database' => 'surreyacuksite',
    'username' => 'surreyacuksite',
    'password' => 'uos3',
    'host' => 'localhost',
    'collation' => 'utf8_general_ci',
    );


