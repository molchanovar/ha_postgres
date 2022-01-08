Vagrant.configure(2) do |config|
  config.vm.box = "ubuntu/bionic64"

  config.vm.provider "virtualbox" do |v|
    v.memory = 2048
    v.cpus = 2
  end

  config.vm.define "postgres1" do |psql1|
    psql1.vm.network :private_network, ip: "192.168.1.11", virtualbox__intnet: "net1"
    psql1.vm.network :forwarded_port, guest: 22, host: 2701, id: "ssh"
    psql1.vm.network :forwarded_port, guest: 80, host: 9001, id: "http"
    psql1.vm.hostname = "postgres1"
  end

  config.vm.define "postgres2" do |psql2|
    psql2.vm.network :private_network, ip: "192.168.1.12", virtualbox__intnet: "net1"
    psql2.vm.network :forwarded_port, guest: 22, host: 2702, id: "ssh"
    psql2.vm.network :forwarded_port, guest: 80, host: 9002, id: "http"
    psql2.vm.hostname = "postgres2"
  end

  config.vm.define "haproxy" do |hpxy|
    hpxy.vm.network :private_network, ip: "192.168.1.13", virtualbox__intnet: "net1"
    hpxy.vm.network :forwarded_port, guest: 22, host: 2703, id: "ssh"
    hpxy.vm.network :forwarded_port, guest: 7000, host: 9003, id: "web_gui"
    hpxy.vm.hostname = "haproxy"
  end

  config.vm.define "etcd" do |etcd|
    etcd.vm.network :private_network, ip: "192.168.1.14", virtualbox__intnet: "net1"
    etcd.vm.network :forwarded_port, guest: 22, host: 2704, id: "ssh"
    etcd.vm.network :forwarded_port, guest: 80, host: 9004, id: "http"
    etcd.vm.hostname = "etcd"
  end

  config.vm.define "ansible" do |a|
    a.vm.network :private_network, ip: "192.168.1.15", virtualbox__intnet: "net1"
    a.vm.network :forwarded_port, guest: 22, host: 2700, id: "ssh"
    a.vm.hostname = "ansible"
    a.vm.provision "shell", run: "always", inline: <<-SHELL
sudo apt-add-repository ppa:ansible/ansible    
sudo apt-get update && sudo apt-get install python3-pip python3-setuptools python3-wheel --yes --quiet && pip3 install ansible
        SHELL
  end

  config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
  config.vm.provision "shell", run: "always", inline: <<-SHELL
sudo sed -i "s/.*PasswordAuthentication\ no/PasswordAuthentication\ yes/g" /etc/ssh/sshd_config
sudo systemctl restart sshd
sudo cat /home/vagrant/.ssh/id_rsa.pub >> /home/vagrant/.ssh/authorized_keys
sudo cat /home/vagrant/.ssh/id_rsa.pub >> /root/.ssh/authorized_keys
sudo chown 600 /home/vagrant/.ssh/id_rsa
        SHELL
end
