Vagrant.configure(2) do |config|
  config.vm.box = "bento/ubuntu-14.04"
  config.vm.synced_folder ".", "/vagrant", type: "nfs"

  1.upto(3) do |n|
    config.vm.define "consul#{n}" do |box|
      box.vm.hostname = "consul#{n}"
      box.vm.network "private_network", ip: "172.20.21.1#{n}"

      box.vm.provision "shell", inline: <<-SCRIPT
sudo apt-get update
sudo apt-get install -y ruby ruby-dev unzip
sudo gem install diplomat --no-rdoc --no-ri
sudo gem install pry --no-rdoc --no-ri

cd /tmp
wget https://releases.hashicorp.com/consul/0.6.4/consul_0.6.4_linux_amd64.zip -O consul.zip

unzip consul.zip
sudo chmod +x consul
sudo mv consul /usr/bin/consul

sudo cp /vagrant/init-consul.conf /etc/init/consul.conf
echo "exec consul agent -config-file=/vagrant/consul.json -advertise=172.20.21.1#{n}" | sudo tee -a /etc/init/consul.conf

sudo mkdir /var/lib/consul
sudo service consul start
      SCRIPT
    end
  end
end
