# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure('2') do |config|

  config.vm.define "lab-test01" do |config|
    config.vm.hostname = "lab-test01"
    add_groups(config.vm.hostname, ['local', 'test'])
    add_disk(config, 5)
    add_disk(config, 5)
  end

end
