$script = <<-SCRIPT
sudo apt-get -y install build-essential libssl-dev libffi-dev python3-dev python3-pip apt-transport-https
pip3 install --user ansible
SCRIPT

Vagrant.configure('2') do |config|
 
    config.vm.define "webnode" do |webnode|
      webnode.vm.box = "debian/stretch64"
      webnode.disksize.size = "10GB"
      webnode.vm.hostname = "webnode"
      webnode.vm.provider "virtualbox" do |webnode|
        webnode.memory = 2048
        webnode.cpus=2
      end 
      config.vm.network "forwarded_port", guest: 80, host: 80
      config.vm.network "forwarded_port", guest: 8888, host: 8888
    end

    config.vm.provision "file", source: "./playbook", destination: "~/playbook"
    config.vm.provision "file", source: "./www", destination: "~/www"

    config.vm.provision "shell", inline: $script

    config.vm.provision 'ansible_local' do |ansible|
      ansible.become = true
      ansible.playbook = '/home/vagrant/playbook/webnode.yaml'
      #ansible.playbook = '/home/vagrant/playbook/update_php.yaml'
      ansible.verbose = 'v'
    end
end    