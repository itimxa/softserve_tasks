#-*- mode: ruby -*-
#vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  
  config.vm.box = "bento/centos-7.4"
  config.vm.box_check_update = false
  config.ssh.private_key_path = ['/home/vadim/.ssh/id_rsa', '~/.vagrant.d/insecure_private_key']
  config.ssh.insert_key = false

  config.vm.define "db_vm" do |db|
    db.vm.hostname = 'first'
    db.vm.network :private_network, ip: "192.168.33.10"
    db.vm.provider 'virtualbox' do |vb|
      vb.customize ['modifyvm', :id, '--memory', '256']
    end
    db.vm.provision "ansible" do |ansible|
      ansible.playbook = 'playbookDB.yml'
    end 
  end
  
  config.vm.define 'app_vm' do |app|
    app.vm.hostname = 'app'
    app.vm.network :private_network, ip: "192.168.33.12"
    app.vm.provider 'virtualbox' do |vb|
      vb.customize ['modifyvm', :id, '--memory', '1024']
    end
    app.vm.provision "ansible" do |ansible|
      ansible.playbook = 'playbookAPP.yml'
    end
    app.vm.provision 'shell',
      inline: "python /vagrant/check.py"
  end

  config.vm.provision "file", source: '/home/vadim/.ssh/id_rsa.pub', destination: '~/.ssh/authorized_keys'
end