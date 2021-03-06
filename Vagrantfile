# -*- mode: ruby -*-
# vi: set ft=ruby :

# Specify Vagrant version and Vagrant API version
Vagrant.require_version '>= 1.6.0'
VAGRANTFILE_API_VERSION = '2'

# Require 'yaml' module
require 'yaml'

# Read YAML file with VM details (box, CPU, and RAM)
machines = YAML.load_file(File.join(File.dirname(__FILE__), 'machines.yml'))

# Create and configure the VMs
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Always use Vagrant's default insecure key
  config.ssh.insert_key = false

  # Iterate through entries in YAML file to create VMs
  machines.each do |machine|
    config.vm.define machine['name'] do |srv|

      # Don't check for box updates
      srv.vm.box_check_update = false
      srv.vm.hostname = machine['name']

      # Set box to VirtualBox box by default
      srv.vm.box = machine['vb_box']

      # Disable default synced folder
      srv.vm.synced_folder '.', '/vagrant', disabled: true

      # Configure the VM with RAM and CPUs per machines.yml (VirtualBox)
      srv.vm.provider 'virtualbox' do |vb|
        vb.memory = machine['ram']
        vb.cpus = machine['vcpu']
        override.vm.box = machine['vb_box']
      end # srv.vm.provider virtualbox

      # Configure VMs with RAM and CPUs per machines.yml (Fusion)
      srv.vm.provider 'vmware_fusion' do |vmw, override|
        vmw.vmx['memsize'] = machine['ram']
        vmw.vmx['numvcpus'] = machine['vcpu']
      end # srv.vm.provider vmware_fusion
    end # config.vm.define
  end # machines.each
end # Vagrant.configure
