# Chef Fundamentals Training Lab VM

- **Date**: 2015/01/19
- **Tools**: Chef-DK, Vagrant, Virtualbox

## Objective

We want to launch an Ubuntu 14.04 VM with Chef Development Kit and Vagrant preinstalled for easy Chef training and lab.

## Usage

		$ vagrant up greenalto/ubuntu-14.04-chefdk --provider virtualbox
		$ vagrant ssh

Check everything works as expected: 

		$ chef verify
		$ vagrant version
		$ vagrant plugin list

### Update box

To update to latest released box version:

		$ vagrant box update

If you want to get rid of previous boxes, they are located under `~/.vagrant.d/boxes/`

## Build

Those are the steps to build manually the lab VM.

### For VirtualBox

Boot it up: 

		$ vagrant up --provider virtualbox

Connect to it: 

		$ vagrant ssh

#### Install Stuff

		$ sudo apt-get update
		$ sudo apt-get upgrade -y
		$ sudo apt-get install vim git build-essential htop -y

#### Install Chef-DK

Find the current version here: [Chef Development Kit Home](https://downloads.getchef.com/chef-dk).

		$ wget https://opscode-omnibus-packages.s3.amazonaws.com/ubuntu/12.04/x86_64/chefdk_0.3.5-1_amd64.deb
		$ sudo dpkg -i chefdk_0.3.5-1_amd64.deb
		$ rm -rf chefdk_0.3.5-1_amd64.deb

#### Configure Chef-DK

		$ echo 'eval "$(chef shell-init bash)"' >> ~/.bash_profile

#### Install Vagrant 

		$ wget https://dl.bintray.com/mitchellh/vagrant/vagrant_1.7.2_x86_64.deb
		$ sudo dpkg -i vagrant_1.7.2_x86_64.deb

Install some useful plugins: 

* AWS Plugin

		$ vagrant plugin install vagrant-aws

* Digital Ocean Plugin

		$ vagrant plugin install vagrant-digitalocean

#### Clean VM

		$ sudo apt-get clean
		$ sudo dd if=/dev/zero of=/EMPTY bs=1M
		$ sudo rm -f /EMPTY
		$ cat /dev/null > ~/.bash_history && history -c && exit

#### Package VM

		$ vagrant package --output ubuntu-14.04-chefdk.box

#### Atlas Availability

* Upload Vagrant box to S3
* Grab URL
* Go to Atlas/Vagrant Cloud
* Update Box version + URL

### For VMware

Currently not supported by `vagrant package`.
