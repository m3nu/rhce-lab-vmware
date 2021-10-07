VAGRANTFILE_API_VERSION = "2"

vdiskmanager = '/Applications/VMware\ Fusion.app/Contents/Library/vmware-vdiskmanager'
vdi_dir = "#{File.dirname(__FILE__)}/vagrant-additional-disks"
disk1 = "#{vdi_dir}/disk1.vmdk"
disk2 = "#{vdi_dir}/disk2.vmdk"
disk3 = "#{vdi_dir}/disk3.vmdk"
disk4 = "#{vdi_dir}/disk4.vmdk"
base_box = "generic/rhel8"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Use same SSH key for each machine
  config.ssh.insert_key = false
  config.vm.box_check_update = false

  config.hostmanager.enable = false
  config.hostmanager.manage_host = false
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  # config.hostmanager.include_offline = true

# # Repo Server
# config.vm.define "repo" do |repo|
#   repo.vm.box = "rdbreak/rhel8repo"
# #  repo.vm.hostname = "repo.ansi.example.com"
#   repo.vm.provision :shell, :inline => "sudo sed -i 's/PasswordAuthentication no/PasswordAuthentication yes/g' /etc/ssh/sshd_config; sudo systemctl restart sshd;", run: "always"
#   repo.vm.provision :shell, :inline => "yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm -y; sudo yum install -y sshpass python3-pip python3-devel httpd sshpass vsftpd createrepo", run: "always"
#   repo.vm.provision :shell, :inline => " python3 -m pip install -U pip ; python3 -m pip install pexpect; python3 -m pip install ansible", run: "always"
#   repo.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
#   repo.vm.network "private_network", ip: "192.168.55.199"

#   repo.vm.provider "virtualbox" do |repo|
#     repo.memory = "512"
#   end
# end

# # Node 1
# config.vm.define "node1" do |node1|
#   node1.vm.box = "rdbreak/rhel8node"
# #  node1.vm.hostname = "node1.ansi.example.com"
#   node1.vm.network "private_network", ip: "192.168.55.201"
#   node1.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
#   node1.vm.provider "virtualbox" do |node1|
#     node1.memory = "1024"

#     unless File.exist?(file_to_disk1)
#       node1.customize ['createhd', '--filename', file_to_disk1, '--variant', 'Fixed', '--size', 2 * 1024]
#       node1.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
#       node1.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk1]
#       end
#   end
  
#     node1.vm.provision "shell", inline: <<-SHELL
#     yes| sudo mkfs.ext4 /dev/sdb
#     SHELL
#     node1.vm.synced_folder ".", "/vagrant"
#  end

# # Node 2
# config.vm.define "node2" do |node2|
#   node2.vm.box = "rdbreak/rhel8node"
# #  node2.vm.hostname = "node2.ansi.example.com"
#   node2.vm.network "private_network", ip: "192.168.55.202"
#   node2.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
#   node2.vm.provider "virtualbox" do |node2|
#     node2.memory = "1024"

#     unless File.exist?(file_to_disk2)
#       node2.customize ['createhd', '--filename', file_to_disk2, '--variant', 'Fixed', '--size', 2 * 1024]
#       node2.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
#       node2.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk2]
#       end
#  end
 
#     node2.vm.provision "shell", inline: <<-SHELL
#     yes| sudo mkfs.ext4 /dev/sdb
#     SHELL
#     node2.vm.synced_folder ".", "/vagrant"
# end

# # Node 3
# config.vm.define "node3" do |node3|
#   node3.vm.box = "rdbreak/rhel8node"
# #  node3.vm.hostname = "node3.ansi.example.com"
#   node3.vm.network "private_network", ip: "192.168.55.203"
#   node3.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
#   node3.vm.provider "virtualbox" do |node3|
#     node3.memory = "512"

#    unless File.exist?(file_to_disk3)
#       node3.customize ['createhd', '--filename', file_to_disk3, '--variant', 'Fixed', '--size', 2 * 1024]
#       node3.customize ['storagectl', :id, '--name', 'SATA Controller', '--add', 'sata', '--portcount', 2]
#       node3.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', file_to_disk3]
#       end
#   end
  
#     node3.vm.provision "shell", inline: <<-SHELL
#     yes| sudo mkfs.ext4 /dev/sdb
#     SHELL
#     node3.vm.synced_folder ".", "/vagrant"
# end

# Node 4
config.vm.define "node4" do |node4|
  node4.vm.box = base_box
 node4.vm.hostname = "node4.ansi.example.com"
  node4.vm.network :private_network
  # node4.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
  node4.vm.provider "vmware_desktop" do |vm|
    vm.linked_clone = false  # To allow adding new HDs
    vm.vmx["memsize"] = "512"
    vm.vmx["numvcpus"] = "1"
    vm.ssh_info_public = true
    unless File.exists?( disk4 )
      `#{vdiskmanager} -c -s 5GB -a lsilogic -t 1 #{disk4}`
    end
    vm.vmx['scsi0.present'] = 'TRUE'
    vm.vmx['scsi0:0.present']  = 'TRUE'
    vm.vmx['scsi0:0.filename'] = disk4
    vm.vmx['scsi0:0.redo'] = ''
    vm.vmx['bios.hddOrder'] = "sata0:0"
  end
  
  # node4.vm.provision "shell", inline: <<-SHELL
  #   yes| sudo mkfs.ext4 /dev/sdb
  # SHELL
  node4.vm.synced_folder ".", "/vagrant"
end

  # Control Node
  config.vm.define "control" do |control|
    control.vm.box = base_box
    control.vm.hostname = "control.ansi.example.com"
    control.vm.network :private_network

    # https://www.vagrantup.com/docs/providers/vmware/configuration
    control.vm.provider "vmware_desktop" do |v|
      v.vmx["memsize"] = "512"
      v.vmx["numvcpus"] = "1"
      v.ssh_info_public = true
      v.vmx['scsi0:1.filename'] = disk4
      v.vmx['scsi0:1.present']  = 'TRUE'
      v.vmx['scsi0:1.redo']     = ''
    end
    # control.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
    config.vm.provision :hostmanager

    # control.vm.provision :ansible_local do |ansible|
    #   ansible.playbook = "/vagrant/playbooks/master.yml"
    #   ansible.install = false
    #   ansible.compatibility_mode = "2.0"
    #   ansible.inventory_path = "/vagrant/inventory"
    #   ansible.config_file = "/vagrant/ansible.cfg"
    #   ansible.limit = "all"
    # end
  end
end