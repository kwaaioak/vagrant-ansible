# Vagrant/Ansible Example

## Running a local (vagrant) environment

### One-time pre-requisites

You'll need to install:

* git (maybe with a GUI such as [SourceTree](https://www.sourcetreeapp.com))
* Vagrant (http://vagrantup.com)
* VirtualBox (http://www.virtualbox.org) or Parallels (http://www.parallels.com)

### Host name resolution

The vagrant virtual machine needs an IP address that does not conflict with any
other virtual machines you have running. Adding an IP address to your /etc/hosts
file serves both to configure this IP address and resolves a convenient hostname
when browsing the server.

Add an entry to your /etc/hosts file that is in the valid host-only range for
your virtual machine hypervisor and does not conflict with any other virtual
machines you have running. Assuming you haven't changed any of your default
networking setup, then one of the following should work for you:

For VirtualBox:

    10.11.12.128     local.example.com

For Parallels:

    10.37.129.128    local.example.com

### Running the application

1. Check out source code (you must have a git account and valid SSH key):

    ```
    $ git clone https://github.com/kwaaioak/vagrant-ansible.git
    ```

2. Start the Virtual Machine

    ```
    $ cd vagrant-ansible
    $ vagrant up
    ```

3. Once running, the server can be reached at local.example.com
