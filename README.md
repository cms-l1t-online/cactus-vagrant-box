# cactus-vagrant-box
Vagrant box instrutctions for L1 Online Software (cactus)

## General information
 - https://svnweb.cern.ch/trac/cactus/wiki/DevInstructions#NightlyBuildMachineBootstrapconfiguration
 - https://twiki.cern.ch/twiki/bin/view/CMS/TrgSupDevGuide1dotxx#Installation
 - https://svnweb.cern.ch/trac/cactus/wiki/DevInstructions

```
svn co --depth immediates  https://svn.cern.ch/reps/cactus/trunk cactus 
cd cactus
svn update --set-depth=infinity cactuscore config scripts
```

## How to make a vagrant box for cactus
A list of general boxes can be found on http://www.vagrantbox.es .

```
# go into your project area
vagrant box add SL6-minimal http://lyte.id.au/vagrant/sl6-64-lyte.box
# modify RAM to whatever you need in the boxes vagrant file (under ~/.vagrant.d/boxes/SL6-minimal)
vb.customize ["modifyvm", :id, "--memory", "1024”]
# cd into the project directory
# initialise the VM (this will create Vagrantfile which you can use to customise further)
vagrant init SL6-minimal
# start it (this will create a private copy of your box in the folder you execute it from)
vagrant up
# you can now log onto the VM with
vagrant ssh
# your project files will be available under /vagrant on the VM
# update the OS
sudo yum update -y
# get the nightly repos
# micro-HAL
wget https://copy.com/8rvgtuALTJJh/cactus/vagrant/uhal-nightly.repo
sudo cp uhal-nightly.repo /etc/yum.repos.d/uhal-nightly.repo
# trigger supervisor
wget https://copy.com/8rvgtuALTJJh/cactus/vagrant/ts-nightly.repo
sudo cp ts-nightly.repo /etc/yum.repos.d/ts-nightly.repo
# XDAQ
wget https://copy.com/8rvgtuALTJJh/cactus/vagrant/xdaq.repo
sudo cp xdaq.repo /etc/yum.repos.d/xdaq.repo
# WANDISCO SVN (SVN 1.8)
wget https://copy.com/8rvgtuALTJJh/cactus/vagrant/wandisco.repo
sudo cp wandisco.repo /etc/yum.repos.d/wandisco.repo
# install command-line editor of your choice and repository software
sudo yum install nano svn git unzip python-devel -y
# install micro-HAL and Trigger Supervisor (TS)
# if you have a previous version of uhal or TS please remove it first:
# sudo yum groupremove uhal triggersupervisor 
sudo yum groupinstall uhal triggersupervisor -y
# needed for xdaq Finite State Machine
sudo yum install daq-toolbox-devel -y
exit
```


Now the box is ready to be used and/or packed.
### Using the vagrant box
```
# cd into your project area
cd <project area>
# start vagrant
vagrant up
# ssh into the box
vagrant ssh
# you might need to set the library path
export LD_LIBRARY_PATH:/opt/cactus/lib:$LD_LIBRARY_PATH
# check if the installed uhal can find all libraries:
ldd /opt/cactus/lib/libcactus_uhal_uhal.so
# there should be no entries with 'not found'
```

### Packing the box
UNDER CONSTRUCTION
```
# stop the box
vagrant halt
# create a new box package
vagrant package default --output cactus.box --vagrantfile Vagrantfile
```
## TODO:
To prepare:
 - [ ] vagrant box for SL6
 - [ ] install cactus dependencies
 - [ ] compile swatch
 - [ ] in parallel: write instructions
