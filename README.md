Vagrant-Ansible Example
=======================

Example project for running Ansible, inside a newly created virtual machine.

For the Windows equivalent, see [here](https://github.com/magenta-aps/vagrant-ansible-example-windows)

## Installation:

To clone this repository recursively, please run:

     git clone --recursive git@github.com:magenta-aps/vagrant-ansible-example.git

Or using older versions of git:

     git clone git@github.com:magenta-aps/vagrant-ansible-example.git
     cd vagrant-ansible-example
     git submodule update --init --recursive

## Setup:

This repository is only meant as a demonstration of how to use Vagrant and
Ansible to setup projects in a uniform way, and thus should not be integrated
directly into sub-projects, rather the following approach should be followed.

To set up a project to use Vagrant and Ansible, first include the vagrant
submodule:

    git submodule add git@github.com:magenta-aps/vagrant.git

Next setup an Ansible folder, the folder from this repository may be used as a
template. Please refer to [README.md in ansible/](ansible/README.md) for
details.

## Usage:

After the setup is complete, please refer to [README.md in vagrant/](https://github.com/magenta-aps/vagrant/blob/master/README.md)
for detail on how to setup your machine to run Vagrant, Ansible and a Hypervisor
of your choice.

Now a virtual machine can be started by simply running:

    cd vagrant
    vagrant up

Once the machine is available, it can be accessed over ssh, using:

    vagrant ssh

If you wish to rerun the provisioning (Shell/Ansible), it can be done using:

    vagrant provision

### Running specific playbooks:

By default the `default.yml` playbook is run, but any playbook can be run, by
changing the `PLAYBOOK` environmental variable before running `vagrant provision`,
as done by:

    PLAYBOOK=demo.yml vagrant provision
