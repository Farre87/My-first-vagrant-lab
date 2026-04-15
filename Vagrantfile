

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

  # --- CLIENT VM ---
  config.vm.define "client" do |client|
    client.vm.hostname = "client-pc"
    client.vm.network "private_network", ip: "192.168.56.13"
    
    # Port forwarding: oppna port 8080 pa din fysiska dator 
    # och skicka trafiken till port 80 pa denna virtuella maskin.
    client.vm.network "forwarded_port", guest: 80, host: 8080

    client.vm.provider "virtualbox" do |vb|
       vb.memory = 1024
       vb.cpus = 1
    end

    # Brandvagg (UFW)
    client.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y ufw apache2
      # Tillat SSH (port 22) 
      ufw allow 22/tcp
      # Tillat webbtrafik
      ufw allow 80/tcp
      # Aktivera brandvaggen
      ufw --force enable
    SHELL
  end
end
