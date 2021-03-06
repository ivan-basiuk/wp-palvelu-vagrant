# -*- mode: ruby -*-
# vi: set ft=ruby :

require 'yaml'

ANSIBLE_PATH = '.' # path targeting Ansible directory (relative to Vagrantfile)

Vagrant.require_version '>= 1.5.1'

Vagrant.configure('2') do |config|
  config.vm.box = 'ubuntu/trusty64'

  config.vm.provision :ansible do |ansible|
    ansible.playbook = File.join(ANSIBLE_PATH, 'site.yml')
    ansible.groups = {
      'web' => ['default'],
      'development' => ['default']
    }
    ansible.extra_vars = {
      ansible_ssh_user: 'vagrant',
      user: 'vagrant'
    }
  end

  config.vm.provider 'virtualbox' do |vb|
    # Give VM access to all cpu cores on the host
    cpus = case RbConfig::CONFIG['host_os']
      when /darwin/ then `sysctl -n hw.ncpu`.to_i
      when /linux/ then `nproc`.to_i
      else 2
    end

    # Customize memory in MB
    vb.customize ['modifyvm', :id, '--memory', 1536]
    vb.customize ['modifyvm', :id, '--cpus', cpus]

    # Fix for slow external network connections
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']
    vb.customize ['modifyvm', :id, '--natdnsproxy1', 'on']
  end
end
