# Ubuntu Server Admin

## Preperation

### Update
Always start by ensuring the server is up to date.

    sudo apt-get update

## Install your software
As an example, I will be installing 'Headphones'.

### Check dependencies
Headphones needs git and python. Check if they are installed already and have the correct versions

    python --version
    git --version

### Install git

    sudo apt-get install git-core

If you havn't done it already, setup git to know your account.

    git config --global user.name "testuser"
    git config --global user.email "testuser@example.com"
    git config --list

You may or may not already have created a ssh key and associated it with your github account. Run `more ~/.ssh/id_rsa.pub` to get the key to paste into the github keys for your account.

Run `ssh-keygen` if the directory doesn't exist or doesn't contain id_rsa.pub

### Install Python

    sudo apt-get install python27

### Create a user account to run headphones
Setting up a homebrew service its convenient to create a user that can log on interactively and use sudo.

To create the user

    sudo adduser headphones

Make sure the user can use `sudo`

    usermod -aG sudo headphones

### Install Headphones

Install headphones locally as the headphones user to ensure the folder privelages are corrent.

    su headphones
    cd ~
    git clone https://github.com/rembo10/headphones.git

Run headphones at least once to create config.ini

    python Headphones.py

Ctrl-C out, edit config.ini and edit http host to be `0.0.0.0', then re-run headphones. You can now access headphones remotely by browsing to

    http://yourserver:8181

### Install Headphones as a Service

The headphones init script is going to look in all the wrong places.
Edit headphones init script settings

    nano /etc/default/headphones

The following settings can be placed in the file. For more look in `headphones/init-scripts/init.ubuntu`

    HP_HOME=/home/headphones/headphones
    HP_DATA=/mnt/media0/headphones
    HP_HOST=0.0.0.0

Now setup the headphones init-script as a service.

* sudo chmod +x ~/headphones/init-scripts/init.ubuntu
* sudo ln -s ~/headphones/init-scripts/init.ubuntu /etc/init.d/headphones
* sudo update-rc.d headphones defaults
* sudo update-rc.d headphones enable
* sudo service headphones [start | stop | reload | restart | status]

Headphones should now be setup as a daemon.

### Install Transmission

This install is going to use the transmission torrent client. Return to your
normal user to install transmission

    sudo apt install transmission-cli transmission-common transmission-daemon

This will have autostarted the transmission daemon, but the default config is
again, going to be less than ideal. Edit the `settings.json` by first stopping the service.

    service transmission-daemon stop
    sudo nano /var/lib/transmission-daemon/info/settings.json
    service transmission-daemon start





