# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/bionic64"
  config.vm.synced_folder "..", "/home/vagrant/ansible-elk"
  
  config.vm.hostname = "elk.local"
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.network :public_network, type: "dhcp"
  config.vm.network :private_network, ip: "192.168.12.50", virtualbox__vboxnet0: true

  config.hostsupdater.remove_on_suspend = true
  config.hostsupdater.aliases = ["alias.elk.local"]

  config.ssh.insert_key = false # 1
    config.ssh.private_key_path = ['~/.vagrant.d/insecure_private_key', '~/.ssh/id_rsa'] # 2
    config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/authorized_keys" # 3

  config.vm.provider :virtualbox do |vb|
  	vb.customize ["modifyvm", :id, "--name", "elk", "--memory", "3048"]
  end

  $script = "sudo apt-get install python -y"
  config.vm.provision :shell, privileged: false, inline: $script

  config.vm.provision :ansible do |ansible|
      ansible.playbook = "vagrant.yml"
    end
end
