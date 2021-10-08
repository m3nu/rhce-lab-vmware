
vdiskmanager = '/Applications/VMware\ Fusion.app/Contents/Library/vmware-vdiskmanager'
vdisk_dir = "#{File.dirname(__FILE__)}/vagrant-additional-disks"

machines=[
  {
    :hostname => "node1",
    :ram => 512,
    :cpu => 1,
    :disk => "5GB",
  },
  {
    :hostname => "node2",
    :ram => 512,
    :cpu => 1,
    :disk => "10GB"
  }
]

Vagrant.configure("2") do |config|
  # Use default insecure SSH key
  config.ssh.insert_key = false
  config.vm.box_check_update = false

  config.hostmanager.enable = false
  config.hostmanager.manage_host = false
  config.hostmanager.manage_guest = true
  config.hostmanager.ignore_private_ip = false
  # config.hostmanager.include_offline = true

  # Worker nodes
  machines.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = 'almalinux/8'
      node.vm.hostname = machine[:hostname]
      node.vm.network :private_network
      # node4.vm.synced_folder ".", "/vagrant", type: "rsync", rsync__exclude: [".git/", "*.vdi"]
      
      node.trigger.after :destroy do |trigger|
        trigger.run = {inline: "bash -c 'rm -rf #{vdisk_dir}/#{machine[:hostname]}*'"}
      end

      node.vm.provider "vmware_desktop" do |vm|
        vm.vmx["memsize"] = machine[:ram]
        vm.vmx["numvcpus"] = machine[:cpu]
        vm.ssh_info_public = true
        
        vm.linked_clone = false  # To allow adding new HDs
        vdisk_path = "#{vdisk_dir}/#{machine[:hostname]}.vmdk"
        unless File.exists?( vdisk_path )
          `#{vdiskmanager} -c -s #{machine[:disk]} -a lsilogic -t 1 #{vdisk_path}`
        end
        vm.vmx['scsi0.present'] = 'TRUE'
        vm.vmx['scsi0:1.present']  = 'TRUE'
        vm.vmx['scsi0:1.filename'] = vdisk_path
        vm.vmx['scsi0:1.redo'] = ''
        # vm.vmx['bios.hddOrder'] = "sata0:0"
      end
      node.vm.provision "shell", inline: <<-SHELL
        # Enable firewall (as is default on RHEL)
        systemctl enable --now firewalld
    SHELL
    end
  end

  # Control Node
  config.vm.define "control" do |control|
    control.vm.box = "generic/rhel8"
    control.vm.hostname = "control"
    control.vm.network :private_network

    # https://www.vagrantup.com/docs/providers/vmware/configuration
    control.vm.provider "vmware_desktop" do |vm|
      vm.vmx["memsize"] = "512"
      vm.vmx["numvcpus"] = "1"
      vm.ssh_info_public = true
    end

    control.vm.provision "shell", privileged: false, inline: <<-SHELL
      # Install insecure default key to reach other nodes
      umask 077
      curl -LJ -o $HOME/.ssh/id_rsa https://raw.githubusercontent.com/hashicorp/vagrant/master/keys/vagrant
    SHELL
  end

  config.vm.provision :hostmanager
end