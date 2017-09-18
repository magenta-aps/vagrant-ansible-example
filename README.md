Vagrant-Ansible Example
=======================

Example project for running Ansible, inside a newly created virtual machine.

## Installation:

Clone this project:

    git clone https://github.com/magenta-aps/vagrant-ansible-example.git


### Ubuntu Xenial (16.04)

To install vagrant, run:

    apt-get update
    apt-get install vagrant

To install Ansible, run:

    apt-get update
    apt-get install ansible
    
Additionally a hypervisor must be installed, the default is VirtualBox:

    apt-get update
    apt-get install virtualbox
    
Other hypervisors can be installed instead, refer to the FAQ for this.


### MacOS Sierra (10.12.3)

Both vagrant and Ansible can be installed using the [Homebrew](https://brew.sh/) CLI.

Vagrant depends on [Virtualbox](https://www.virtualbox.org/) for MacOS.
To install vagrant and Virtualbox, add the 'cask' tap to homebrew and run:

    brew tap caskroom/cask
    brew install caskroom/cask/virtualbox
    brew install caskroom/cask/vagrant

To install Ansible, run:

    brew install ansible

### Windows 10 Home

To install vagrant;

1. Download the msi file from [here](https://releases.hashicorp.com/vagrant/2.0.0/vagrant_2.0.0_x86_64.msi)
2. Install it using the default Windows method of clicking 'Next' and accepting everything blindly.

To install Ansible;

1. Windows isn’t supported for the control machine.
    
Additionally a hypervisor must be installed, on Windows we use VirtualBox;

1. Download the exe file from [here](http://download.virtualbox.org/virtualbox/5.1.28/VirtualBox-5.1.28-117968-Win.exe)
2. Install it using the default Windows method of clicking 'Next' and accepting everything blindly.
    
No other hypervisors are supported.

### Other platforms

Not supported, please figure out a solution yourself, and add it via. a pull
request.


## Usage:

Simply run `vagrant up`, and wait for the machine to be available.

After this, the machine can be accessed over ssh, using:

    vagrant ssh

If you wish to rerun the provisioning (Ansible), it can be done using:

    vagrant provision

## FAQ:

### I can't serve HTTP from guest to host

Could be related to using VirtualBox. When using Vagrant with VirtualBox, you'll be prompted to choose a bridged network interface. Select the interface that is being used to connect to the internet.

Also, try to forward ports in your `Vagrantfile` by adding this line:

    config.vm.network "forwarded_port", guest: 8000, host: 8000
    

### What if I don't like VirtualBox?

Fortunately several other providers can be utilized, specifically:
* `lxc`
* `libvirt`

Some provider specific setup is required however. The approach below is
confirmed to work on Ubuntu 16.04 (Xenial).

##### LXC

Install the system dependencies, using:

    apt-get update
    apt-get install lxc lxc-templates cgroup-lite redir

Install the vagrant lxc plugin, using:

    vagrant plugin install vagrant-lxc

The expected output is:

    Installing the 'vagrant-lxc' plugin. This can take a few minutes...
    Installed the plugin 'vagrant-lxc (1.2.3)'!

If the expected result is different, please check the Quirks section below.

##### Libvirt

Install the system dependencies, using:

    apt-get update
    apt-get build-dep vagrant ruby-libvirt
    apt-get install qemu libvirt-bin ebtables dnsmasq
    apt-get install libxslt-dev libxml2-dev libvirt-dev zlib1g-dev ruby-dev

Install the vagrant lxc plugin, using:

    vagrant plugin install vagrant-libvirt

The expected output is:

    Installing the 'vagrant-libvirt' plugin. This can take a few minutes...
    Installed the plugin 'vagrant-libvirt (0.0.40)'!

If the expected result is different, please check the Quirks section below.


## Quirks:
### Installing non-virtualbox provider fails.

It is a known issue, that installing the libvirt or LXC proviers, can result in
an issue, alike the one below:

    Installing the 'vagrant-lxc' plugin. This can take a few minutes...
    /usr/lib/ruby/2.3.0/rubygems/specification.rb:946:in `all=':
        undefined method `group_by' for nil:NilClass (NoMethodError)
    ...

The solution to this issue, is running code of the internet:

    sed -i'' "s/Specification.all = nil/Specification.reset/" \
        /usr/lib/ruby/vendor_ruby/vagrant/bundler.rb


