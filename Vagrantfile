# -*- mode: ruby -*-
# vi: set ft=ruby :

GROUPS = {}
DISKS = {}

# Add host in the specified groups.
#
# ==== Attributes
# * +host+ - Hostname
# * +groups+ - Groups to assign
#
# ==== Examples
#
# add_groups('pa2-web01', ['local', 'web'])
# add_groups('pa2-sql01', ['local', 'mysql', 'heartbeat'])
def add_groups(host, groups=[])
    for group in groups
        if !GROUPS.key?(group) then
            GROUPS[group] = []
        end
        GROUPS[group].push(host)
    end
end

# Add disk to the virtual machine
#
# ==== Attributes
# * +server+ - Server object
# * +size+ - Disk size in bytes
#
# ==== Examples
#
# add_disk(server, 5) # Add a 5GB disk
# add_disk(server, 10)  # Add a 10GB disk
def add_disk(server, size)
    host = server.to_s

    # Increment disk id
    if !DISKS.key?(host) then
        DISKS[host] = 0
    else
        DISKS[host] += 1
    end
    disk_id = DISKS[host]
    disk_filename = ".vagrant/disks/" + host + "_" + disk_id.to_s + ".vdi"

    server.vm.provider "virtualbox" do |v|
      # Create disk if it not exist
      unless File.exist?(disk)
        v.customize ["createhd",  "--filename", disk_filename, "--size", size * 1024 * 1024]
      end
      v.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', disk_id, '--device', 0, '--type', 'hdd', '--medium', disk]
    end
end

Vagrant.configure('2') do |config|

  # Use Debian Jessie as default operating system
  config.vm.box = 'debian/jessie64'

  # Disable synced folder
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Execute install.yml playbook
  config.vm.provision "main", type: "ansible" do |ansible|
    ansible.groups = GROUPS

    # Playbook to execute during server provisionning
    ansible.playbook = "install.yml"

    # You can specified vault password file
    #ansible.vault_password_file = "vault_password.txt"
  end

end

# Include all .Vagrantfile files from vagrant.d/
vagrantfiles = Dir["vagrant.d/*.Vagrantfile"]
vagrantfiles.each do |vagrantfile|
  load File.expand_path(vagrantfile)
end