IMATGE_BOX = "debian/bookworm64"
NOM_NODE = "k8s"
MEMORIA = 4096
CPUS = 4
TARGETA_XARXA = "enp8s0f1"

Vagrant.configure("2") do |config|
  config.vm.box = IMATGE_BOX
  config.vm.hostname = NOM_NODE
  config.vm.network "public_network", bridge: TARGETA_XARXA
  config.vm.provider "virtualbox" do |v|
     v.name = NOM_NODE
     v.memory = MEMORIA
     v.cpus = CPUS
     v.customize ['modifyvm', :id, '--clipboard', 'bidirectional', '--groups', '/K8S']     
  end
 
  config.vm.provision "shell", inline: <<-SHELL
    sudo apt-get update -y
    sudo apt-get install -y net-tools
    sudo apt-get install -y whois
    sudo apt-get -y install apt-transport-https ca-certificates curl gnupg2 software-properties-common
    curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"
    sudo apt-get update -y
    sudo sudo apt-get -y install docker-ce docker-ce-cli containerd.io docker-compose
    sudo gpasswd -a vagrant docker
    sudo apt-get install -y snapd
    echo 'export PATH=/snap/bin:$PATH' >> /home/vagrant/.bashrc
    source /home/vagrant/.bashrc
    sudo snap install microk8s --classic
    sudo gpasswd -a vagrant microk8s
    sudo  chown -f -R vagrant /home/vagrant/.kube
    echo "alias kubectl='microk8s kubectl'" >> /home/vagrant/.bashrc
    source /home/vagrant/.bashrc    
    exit
  SHELL
end
