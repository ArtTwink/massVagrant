# -*- mode: ruby -*-
# # # vi: set ft=ruby :

$box ||= "generic/ubuntu1804"
$subnet ||= "172.16.10"
$hostname_prefix ||= "test"
$num_instances ||= 2
$vm_memory ||= 1024
$vm_cpus ||= 2
$libvirt_nested ||= false

Vagrant.configure("2") do |config|
  config.vm.box = $box
  (1..$num_instances).each do |i|
    config.vm.define "node-#{i}" do |node|
      
      #FOR VirtualBOX
      node.vm.provider :virtualbox do |vb|
        vb.memory = $vm_memory
        vb.cpus = $vm_cpus
        vb.gui = false
        vb.customize ["modifyvm", :id, "--vram", "8"] #ubuntu defaults to 256MB which is a waste of precious RAM
      end

      #FOR LIBVIRT
      node.vm.provider :libvirt do |lv|
        lv.nested = $libvirt_nested
        lv.memory = $vm_memory
        lv.cpus = $vm_cpus
        lv.default_prefix = $hostname_prefix
      end
      
      node.vm.provision "shell",
        inline: <<-SHELL
        
        echo hello from node #{i}
        
        swapoff -a

        sed -i 's/ChallengeResponseAuthentication no/ChallengeResponseAuthentication yes/g' /etc/ssh/sshd_config    
        service ssh restart

        SHELL

  
      ip = "#{$subnet}.#{i+100}"
      node.vm.network :private_network, ip: ip
  
      node.vm.hostname = $hostname_prefix
#      node.ssh.insert_key = false
#      node.ssh.keys_only = false
#      node.ssh.private_key_path = "~/.ssh/id_rsa"
#
    end
  end
end
