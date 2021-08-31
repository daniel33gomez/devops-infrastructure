Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.disk :disk, size: "50GB", primary: true
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.box_check_update = true

  # Ports for sonarqube
  config.vm.network "forwarded_port", guest: 9000, host: 9000
  # Ports for jenkins
  config.vm.network "forwarded_port", guest: 8080, host: 8090
  config.vm.network "forwarded_port", guest: 50000, host: 50000
  # Ports for nexus
  config.vm.network "forwarded_port", guest: 8081, host: 8081
  # Ports fot the project
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "DevOpsInfrastructrureVM"
    vb.cpus = 2
    vb.memory = "8192"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release -y
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
    echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
    sudo apt-get update
    sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose -y
    sudo apt install git
    sudo -i
    echo "vm.max_map_count=262144" >> /etc/sysctl.conf
    git clone https://github.com/daniel33gomez/devops-infrastructure.git
    cd devops-infrastructure
    docker-compose -f docker-compose.yml up -d 
    
  SHELL
end
