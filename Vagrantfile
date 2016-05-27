# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant vagranturation is done below. The "2" in Vagrant.vagranture
# vagrantures the vagranturation version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|

  # Fuer vagrant script Ausgaben
  ui = Vagrant::UI::Colored.new

  # The most common vagranturation options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.
  # VirtualBox Configuration

  machines = {
               :dh1     => {:ip => '192.168.56.21', :box => 'ubuntu/trusty64', :mem => '1024', :cpu => 1 },
               :dh2     => {:ip => '192.168.56.22', :box => 'ubuntu/trusty64', :mem => '1024', :cpu => 1 },
               :dh3     => {:ip => '192.168.56.23', :box => 'ubuntu/trusty64', :mem => '1024', :cpu => 1 },
             }

  machines.each do |machine_name, machine_details|
   config.vm.define machine_name do |machine_config|
     machine_ip = machine_details[:ip]
     ui.info("Handling vm with hostname [#{machine_name.to_s}] and IP [#{machine_ip}]")
     machine_config.vm.box = machine_details[:box]
     machine_config.vm.network :private_network, ip: machine_ip
     machine_config.vm.hostname = machine_name.to_s
     machine_config.vm.provider :virtualbox do |vb|
       reserved_mem = machine_details[:mem] || default_mem
       reserved_cpu = machine_details[:cpu] || default_cpu
       vb.name = machine_name.to_s
       vb.customize ["modifyvm", :id, "--memory", reserved_mem]
       vb.customize ["modifyvm", :id, "--cpus", reserved_cpu]
       vb.gui = false
       vb.customize ["setextradata", :id, "VBoxInternal2/SharedFoldersEnableSymlinksCreate/v-root", "1"]
     end #vb
     machine_config.vm.provision "shell", inline: "apt-get install python"
   end # machine_config
  end # machines

  config.vm.define "master" do |vm_config|
    vm_config.vm.box = "ubuntu/trusty64"
    vm_config.vm.hostname = "master"
    vm_config.vm.network :private_network, ip: "192.168.56.10"
    vm_config.vm.provider :virtualbox do |vb|
      vb.name = "master"
      vb.customize ["modifyvm", :id, "--memory", 512]
      vb.customize ["modifyvm", :id, "--cpus", 1]
      vb.gui = false
    end
    vm_config.vm.provision "shell", path: "scripts/ansible.sh"
  end

end
