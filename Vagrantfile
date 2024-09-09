# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

$script = <<SCRIPT
yes | ssh-keygen -b 2048 -t rsa -f /home/vagrant/.ssh/id_rsa -q -N "" -C vagrant@ansible
cd /home/vagrant/.ssh/
sudo chown vagrant:vagrant id_rsa id_rsa.pub
cp /home/vagrant/.ssh/id_rsa.pub /vagrant
mkdir -p /home/vagrant/ansible/  && cp -r /vagrant/ansible/* /home/vagrant/ansible/
apt update && yes | apt upgrade
yes | apt install ansible
yes | apt install python3-pip
pip3 install ansible-lint

echo '##########################'
echo '# PROVISION COMPLETE !!! #'
echo '##########################'

SCRIPT

$script2 = <<SCRIPT
cat /vagrant/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys

echo '##########################'
echo '# PROVISION COMPLETE !!! #'
echo '##########################'

SCRIPT

Vagrant.configure("2") do |config|
    # for all
    config.vm.synced_folder ".", "/vagrant"

    config.vm.define "ansible" do |ansible|
        ansible.vm.box = "ubuntu/jammy64"
        ansible.vm.hostname = "ansible"
        ansible.vm.network "private_network", ip: "192.168.10.20", adapter: 2, netmask: "255.255.255.0", virtualbox_inet: "vmnet"
        ansible.vm.provider :virtualbox do |vm|
            vm.name = "ansible"
            vm.memory = "2048"
            vm.cpus = 1
        end
        ansible.vm.provision "shell" do |script|

            script.inline = $script
        end
    end

    config.vm.define "systemd" do |systemd|
        systemd.vm.box = "ubuntu/jammy64"
        systemd.vm.hostname = "systemd"
        systemd.vm.network "private_network", ip: "192.168.10.10", adapter: 2, netmask: "255.255.255.0", virtualbox_inet: "vmnet"
        systemd.vm.provider :virtualbox do |vm|
            vm.name = "systemd"
            vm.memory = "2048"
            vm.cpus = 1
        end
        systemd.vm.provision "shell" do |script|
            script.inline = $script2
        end    
    end
end