

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/jammy64" 

  # Webbserver
  config.vm.define "web" do |web|
    web.vm.hostname = "webserver"
    web.vm.network "private_network", ip: "192.168.56.10"

    web.vm.provider "virtualbox" do |vb|
       vb.memory = 1024
       vb.cpus = 1
    end

    web.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2 curl
      # Automatisera SSH-fixen 
      sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
      systemctl restart ssh
      echo 'vagrant:vagrant' | chpasswd
    SHELL
  end

  # Databasserver
  config.vm.define "db" do |db|
    db.vm.hostname = "database"
    db.vm.network "private_network", ip: "192.168.56.11"

    db.vm.provider "virtualbox" do |vb|
       vb.memory = 1024
       vb.cpus = 1
    end

    db.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y postgresql
      # Samma SSH-fix har
      sed -i 's/^PasswordAuthentication no/PasswordAuthentication yes/' /etc/ssh/sshd_config
      systemctl restart ssh
      echo 'vagrant:vagrant' | chpasswd
    SHELL
  end

  # Kontrollnod for Ansible
  config.vm.define "control" do |control|
    control.vm.hostname = "ansible-control"
    control.vm.network "private_network", ip: "192.168.56.12"

    control.vm.provider "virtualbox" do |vb|
       vb.memory = 1024
       vb.cpus = 1
    end
    
    control.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ansible
    SHELL
  end   
end
