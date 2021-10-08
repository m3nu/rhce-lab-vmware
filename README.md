# RHCE Vagrant Practice Lab (For VMWare Fusion)

This Vagrant file will spin up one control node and multiple worker nodes to use as test environment for Red Hat Certified Engineer exams. Worker nodes receive an optional secondary disk for storage-related exercises.

This is based on previous work by [rdbreak](https://github.com/rdbreak/rhce8env), but adjusted to use VMWare Fusion instead of VirtualBox for better speed.


## Install
- `brew install vagrant vagrant-vmware-utility vagrant-completion`
- `vagrant plugin install vagrant-vmware-desktop vagrant-hostmanager` [Instructions](https://www.vagrantup.com/docs/providers/vmware)


## Usage
- `vagrant up [control|node1|...]`
- Update hosts file to contain all nodes: `vagrant hostmanager`


## Credits
- Based on work by [rdbreak](https://github.com/rdbreak/rhce8env)