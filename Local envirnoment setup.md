# Local envirnoment setup

written on 09/02/2022

## Intro

Welcome! 

This guide is for setting up the local environment on a MacBook Pro with MacOS Monterrey and chip M1 Pro, and running the WordPress Cluster repo locally.

## Install Vagrant

Install it via package manager.
Make sure you have `brew` already installed.

`````bash
$ brew install vagrant
`````

Make sure you added vagrant command to your PATH (??check this), so it can be executed. You'll be promted to do so after the installation finished.

Alternatively, you can installed Vagrant downloading a `dmg` image (https://www.vagrantup.com/downloads)., and the installation process will add vagrant command to the PATH

To check that Vagrant has been successfully installed, run this command:

````bash
$ vagrant	
````

and you should see something like this:

````bash
Usage: vagrant [options] <command> [<args>]	
...	
````



## Install Parallels

Go here https://www.parallels.com/uk/ and select the macOS download option.
It's a `.dmg` file.

You can use the free trial version for a while, but you'll need a license; you can ask for it to Ali or Jason.

Make sure you activate your trial version.



## Install  vagrant plugin

````bash
$ vagrant plugin install vagrant-goodhosts vagrant-parallels
````



## Clonning the WPC repo

Go to https://github.com/dc-thomson/wordpress and clone it using HTTPS credentials. After running this on your terminal:

```
$ git clone https://github.com/dc-thomson/wordpress.git
```

​	you'll be promted to authenticate on GitHub.

After doing that, you'll be able to clone the project.

The repo is mainly composed of PHP files, but not the sql database with data containing posts, media, etc.



## Cloning the WPC database

To run WordPress locally, a sql database is needed, so that's why the WPC database has to be cloned from https://dcthomsongroup-my.sharepoint.com/:u:/g/personal/jason_murray_dcthomson_co_uk/EYReIw9pM55NhxhiVdH2ebgBXfyxAzAzE3P8Quc8iQbyng?e=mpidrl

After downloading and uncompressing the file, rename it to `wpc.sql` and place at the root level of the WPC local project. (???CHECK: should we put directly inside `wordpress/build/VVV/database/sql/backups/` instead??).



## Vagrant configuration

From this point forward, all the commands run on the terminal will be at `wordpress/build/VVV` unless stated.

Open your terminal at `wordpress/build/VVV`, and run the following command:

```
$ git clone -b stable https://github.com/Varying-Vagrant-Vagrants/VVV.git VVV
```

GO to `wordpress/build/VVV/config/config.yml` and replace the content with this:

```yaml
---
​
# This file is a YAML formatted file. YAML indenting is done in spaces not
# tabs, and whitespace is significant. If you don't stick to this, it will
# fail on provision
​
#
# IMPORTANT, if you change this file, you have to reprovision,  no exceptions
# Do this by running either this command:
# vagrant reload --provision
​
# Or, if your machine is already turned on:
# vagrant provision
#
​
# These are your websites, and their names map on to the folders they're
# located in. See the docs for how to define these, and what all the keys
# and options are
sites:
​
  # latest version of WordPress, can be used for client work and testing
  # Check the readme at https://github.com/Varying-Vagrant-Vagrants/custom-site-template
  wordpress-one:
    skip_provisioning: false
    description: "A standard WP install, useful for building plugins, testing things, etc"
    repo: https://github.com/Varying-Vagrant-Vagrants/custom-site-template.git
    hosts:
      - one.wordpress.test
    custom:
      wpconfig_constants:
        WP_DEBUG: true
        WP_DEBUG_LOG: true
        WP_DISABLE_FATAL_ERROR_HANDLER: true # To disable in WP 5.2 the FER mode
​
  wpc:
    vm_dir: /srv/www/wpcluster
    local_dir: ../../
    nginx_upstream: php74
    hosts:
      - wpcluster.test
      - thecourier.test
      - sundaypost.test
      - pressandjournal.test
      - energyvoice.test
    #tideways: true
    custom:
      wp_type: none
      db_name: wpc
      install_plugins:
        - query-monitor
        - debug-bar
        - debug-bar-actions-and-filters-addon
        - debug-bar-elasticpress
      wpconfig_constants:
        WP_DEBUG: true
        WP_DEBUG_LOG: true
        WP_DISABLE_FATAL_ERROR_HANDLER: true # To disable in WP 5.2 the FER mode
​
  wpcguten:
    description: "A WordPress Cluster install with Gutenberg enabled. Bare-bones database."
    vm_dir: /srv/www/wpcguten
    local_dir: ../../
    nginx_upstream: php74
    hosts:
      - wpcguten.test
    #tideways: true
    custom:
      wp_type: none
      db_name: wpcguten
      install_plugins:
        - query-monitor
        - debug-bar
        - debug-bar-actions-and-filters-addon
        - debug-bar-elasticpress
      wpconfig_constants:
        WP_DEBUG: true
        WP_DEBUG_LOG: true
        WP_DISABLE_FATAL_ERROR_HANDLER: true # To disable in WP 5.2 the FER mode
​
# Utilities https://varyingvagrantvagrants.org/docs/en-US/utilities/
# are system level items that aren't websites, that install tools or packages
# the core utilities install tools such as phpmyadmin
utilities:
  core: # The core VVV utility
    - memcached-admin # Object cache management
    - opcache-status # opcache management
    - phpmyadmin # Web based database client
    - webgrind # PHP Debugging
    - tls-ca # SSL/TLS certificates
    #- mongodb # needed for Tideways/XHGui
    #- tideways # PHP profiling tool, also installs xhgui check https://varyingvagrantvagrants.org/docs/en-US/references/tideways-xhgui/
    #- php56
    #- php70
    #- php71
    #- php72
    #- php73
    - php74
​
utility-sources:
  core:
    repo: https://github.com/Varying-Vagrant-Vagrants/vvv-utilities.git
    branch: master
​
​
# vm_config controls how Vagrant provisions the virtual machine, and can be used to
# increase the memory given to VVV and the number of CPU cores.
# It can also be used to override the default provider being used within Vagrant.
​
# DCT - Changes to these values require a reprovision to take effect (`bin/provision`). The edits must be made to the
# /build/VVV/config/config.yml version of the file, rather than the base copy in the /provision folder.
vm_config:
  # DCT - The VM will use up to double the amount of RAM specified here, so do not set this above half
  # of the RAM available to your system. When not using ES or restoring the DB a value of 5120 works well, otherwise
  # at least 8192 is required.
  memory: 8192
  # DCT - Setting this to half the logical cores available to your machine works well. macOS `sysctl hw.logicalcpu`
  cores: 8
  provider: parallels
  box: generic/ubuntu2004
​
# General VVV options
general:
  # Backup the databases to the database/backups subfolder on halt/suspend/destroy, set to false to disable
  db_backup:
      enable: false
      gzip: true
      #exclude:
      #  - wordpress-trunk
  # Import the databases if they're missing from backups
  db_restore: true
  # set to true to use a synced shared folder for MariaDB database storage
  db_share_type: false
  # GitHub token to use from composer
  #github_token:
```

 If you see a `default-config.yml`, delete it.



## Creating a box

Run the following command:

```
$ vagrant up
```

If all went well, you should see a nice bear printed to the terminal:

``````bash
    default: 
    default:   ✧ ▄▀▀▀▄▄▄▄▄▄▄▀▀▀▄ ✧  Thanks for  __ __ __ __
    default:    ✧█▒▒░░░░░░░░░▒▒█    using       \ V\ V\ V /
    default:  ✧   █░░█░░░░░█░░█ ✧                \_/\_/\_/
    default:   ▄▄  █░░░▀█▀░░░█  ▄▄✧ 
    default:  █░░█ ▀▄░░░░░░░▄▀ █░░█ Vagrant Up has finished! Visit http://vvv.test
``````

The box has been created, and it provides a LEMP stack up and running on a virtual machine, so we can run your WordPress cluster project locally.

LEMP stands for Linux (as OS), Nginx (as web server), MySQL (as a database) and PHP (to handle dynamic processing).

Using the Finder, go to `/Users/{your-user-name}` and look the hidden files with this command:

````
cmd + shift + .
````

Then go to `/Users/{your-user-name}/.vagrant.d/boxes/mpasternak-VAGRANTSLASH-focal64-arm/202110.0.0/parallels/Ubuntu Linux.pvm`. , right click and press `Open`. 

## Deleting the snapshot

Run the following command to halt the virtual machine.

````bash
$ vagrant halt
````

Then, open `Parallels` desktop app and go to `Window -> Control Center`. A card will pop up, that says `Ubuntu Linux`.  right click on it, and click on `Manage snapshots`, and delete the snapshots you find there (the snapshot will have a name like `vagrant_linked_clone`). Then, you should see a message saying `No snapshots available`.



## Resizing the virtual disk

As the `wp.sql` database is over 30Gb, we need to resize the virtual disk.

 Go to `Parallels Desktop` app, go to `Window -> Control Center`, right click on the `Ubuntu Linux` card, and select `Configure`. Then go to `Hard Disk -> Advanced -> properties` and change the size to `160Gb`.

⚠️ Important note!: don't lock the padlock when doing changes on `Parallels Desktop`, that leads to an error when running `$ vagrant up` later on.



## Resize the partition table

Go back to the terminal and run this in order to spin up the virtual machine and get into its terminal:

````bash
$ vagrant up
$ vagrant ssh
````

Let's now give the new size of the disk:

````bash
$ cfdisk
````

You'll see a graphic interface to change the size of `/dev/sda3` device. 

To do that, press the down arrow till `/dev/sda3` row is selected, then press the left arrow, so `Resize` option is selected. 

Then press `Return`. You'll be prompted to enter the new size. Enter `160`. Press `Enter`.

 Then press the right arrow till selecting `Write` option, and press `Enter`. Then you'll be prompted with:

````bash
 Are you sure you want to write the partition table to disk?
````

So type `Yes` and then press `Enter`.

The press left arrow to select `Quit` and press `Enter`.

Let's now see a list of... (CHECK This ????) by running this:

```bash
$ df -h
```

You'll see a list of things, like this:

````bash
Filesystem                         Size  Used Avail Use% Mounted on
udev                               3.9G     0  3.9G   0% /dev
tmpfs                              794M  1.1M  793M   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv  32G   10G   22G  47% /
...
````

The important line here is `/dev/mapper/ubuntu--vg-ubuntu--lv  32G   10G   22G  47% /`.

There will be a lot of commands that require admin permission, so let's switch to the super user — or root user:

````bash
$sudo su
````

Then, you should see this:

````bash
root@vvv:/home/vagrant#
````

Now, let's check the size of (is this step necessary CHECK ??):

````bash
$vgdisplay
````

````bash
 --- Volume group ---
  VG Name               ubuntu-vg
  System ID             
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  6
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <32 GiB
  PE Size               4.00 MiB
  Total PE              40575
  Alloc PE / Size       10000 / <32 GiB
  Free  PE / Size       0 / 0   

````

Let's now resize the disk:

```bash
$ pvresize /dev/sda3
```

and resize the logical volume:

````bash
$ lvextend -l +100%FREE /dev/mapper/ubuntu--vg-ubuntu--lv 
$resize2fs /dev/mapper/ubuntu--vg-ubuntu--lv 
````

To check the new size, run:

````bash
$df -h
````

You should see:

````bash
/dev/mapper/ubuntu--vg-ubuntu--lv  156G   69G   81G  47% /
````

Exit the VM's terminal by running:

````bash
$ exit	
````

And run:

````bash
$ vagrant up --provision	
````

It will take a while, because the `wpc.sql` database will be restored.

That's the vagrant + parallels setup done! 🎉



## Running WCP locally

Create a WP user by running this command at `/wordpress`:

````bash
$ bin/wp user create yourUsername yourUsername@example.com
````

You'll be prompted with this:

````
Success: Created user 11826.
Password: !eVzhaXnGBn...
Connection to 10.211.55.3 closed.
````

You'll need to use the generated password to login in a later step.

Then run:

`````bash
$ bin/wp super-admin add yourUsername
`````

You shout get a message like this:

````bash
Success: Granted super-admin capabilities to 1 user.
Connection to 10.211.55.3 closed.
````

Go to https://wpcluster.test/, and enter the wp username and password.

Then, you can access the test pages, like `thecourier.test`.

That's it! 🎉

☎️ if you get stuck, don't hesitate to reach out to Jason, Fabhia or Esteban.





