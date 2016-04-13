# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

application_slug = "example"
application_ram  = 1024
application_cpus = 1

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.hostname = "local.#{application_slug}.com"
  config.vm.box = "ubuntu/trusty64"

  # Application provisioning
  ansible = lambda do |override|
    bootstrap_apt_dependencies = "git python-setuptools python-dev"
    bootstrap_pip_dependencies = "ansible"

    override.vm.provision :shell, inline: "test -x /usr/bin/ansible || ( apt-get update && apt-get install -y #{bootstrap_apt_dependencies} && easy_install pip && pip install #{bootstrap_pip_dependencies} )"
    override.vm.provision :shell, keep_color: true, inline: "! test -s /vagrant/ansible/requirements.yml || ( cd /vagrant/ansible && PYTHONUNBUFFERED=1 ANSIBLE_FORCE_COLOR=true ansible-galaxy install -r requirements.yml -f )"
    override.vm.provision :shell, privileged: false, keep_color: true, inline: "PYTHONUNBUFFERED=1 ANSIBLE_FORCE_COLOR=true ansible-playbook -i /vagrant/ansible/inventories/vagrant/hosts /vagrant/ansible/site.yml"
  end

  # Networking
  begin
    config.vm.network :private_network, ip: Socket::getaddrinfo(config.vm.hostname, 'http', nil, Socket::SOCK_STREAM)[0][3]
  rescue SocketError
    $stderr.puts "Could not resolve " + config.vm.hostname + " (see README.md) - one will be assigned automatically"
  end

  # Parallels
  config.vm.provider :parallels do |p, override|
    p.name = application_slug
    p.memory = application_ram
    p.cpus = application_cpus

    override.vm.box = "parallels/ubuntu-14.04"

    ansible.call override
  end

  # VirtualBox
  config.vm.provider :virtualbox do |vb, override|
    vb.name = application_slug
    vb.memory = application_ram
    vb.cpus = application_cpus
    
    ansible.call override
  end

  # File sharing
  config.vm.synced_folder "./cache/apt/archives", "/var/cache/apt/archives"
  unless RUBY_PLATFORM =~ /cygwin|mswin|mingw|bccwin|wince|emx/
    config.vm.provider :virtualbox do |v, override|
      override.vm.synced_folder ".", "/vagrant", type: "nfs"
      override.vm.synced_folder "./cache/apt/archives", "/var/cache/apt/archives", type: "nfs"
    end
  end

  config.vm.post_up_message = "Server is running and can be found at #{config.vm.hostname}"
end
