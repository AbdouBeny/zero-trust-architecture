Vagrant.configure("2") do |config|
  # Global VM settings
  config.vm.box = "ubuntu/focal64"
  
  # VM1 - Keycloak Server (IAM/SSO)
  config.vm.define "vm1" do |keycloak|
      keycloak.vm.hostname = "keycloak-sso"
      keycloak.vm.network "private_network", ip: "192.168.56.10"
      keycloak.vm.provider "virtualbox" do |vb|
        vb.name = "Keycloak-IAM"
        vb.memory = "1536"
        vb.cpus = 1
      end
      keycloak.vm.provision "ansible" do |ansible|
        ansible.playbook = "ansible/keycloak.yml"
        ansible.inventory_path = "ansible/inventory.ini"
        ansible.limit = "vm1"
        ansible.extra_vars = {
          ansible_ssh_pipelining: true
        }
      end
  end

  
  # VM2 - Application Server
  config.vm.define "vm2" do |app|
    app.vm.hostname = "app-server"
    app.vm.network "private_network", ip: "192.168.56.20"
    app.vm.network "private_network", ip: "10.10.20.10", virtualbox__intnet: "vlan-apps"
    app.vm.provider "virtualbox" do |vb|
      vb.name = "App-Server"
      vb.memory = "1024"  # 1GB
      vb.cpus = 1
    end
    app.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/app_server.yml"
      ansible.inventory_path = "ansible/inventory.ini"
      ansible.limit = "vm2"
      ansible.extra_vars = {
        ansible_ssh_pipelining: true
      }
    end

  end
  
  # VM3 - Zero Trust Gateway
  config.vm.define "vm3" do |gateway|
    gateway.vm.hostname = "zero-trust-gw"
    gateway.vm.network "private_network", ip: "192.168.56.30"
    gateway.vm.network "private_network", ip: "10.10.10.10", virtualbox__intnet: "vlan-dmz"
    gateway.vm.network "private_network", ip: "10.10.20.1", virtualbox__intnet: "vlan-apps"
    gateway.vm.provider "virtualbox" do |vb|
      vb.name = "ZT-Gateway"
      vb.memory = "1024"  # 1GB
      vb.cpus = 1
    end
    gateway.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/gateway.yml"
      ansible.inventory_path = "ansible/inventory.ini"
      ansible.limit = "vm3"
      ansible.extra_vars = {
        ansible_ssh_pipelining: true 
      }
    end
  end
  
  # VM4 - Internal Client
  config.vm.define "vm4" do |internal|
    internal.vm.hostname = "client-internal"
    internal.vm.network "private_network", ip: "192.168.56.40"
    internal.vm.network "private_network", ip: "10.10.10.100", virtualbox__intnet: "vlan-dmz"
    internal.vm.provider "virtualbox" do |vb|
      vb.name = "Client-Internal"
      vb.memory = "1024"
      vb.cpus = 1
      #vb.gui = true
    end
    internal.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/client_internal.yml"
      ansible.inventory_path = "ansible/inventory.ini"
      ansible.limit = "vm4"
      ansible.extra_vars = {
        ansible_ssh_pipelining: true  
      }
    end
  end
  
  # VM5 - Remote VPN Client
  config.vm.define "vm5" do |remote|
    remote.vm.hostname = "client-remote"
    remote.vm.network "private_network", ip: "192.168.56.50"
    remote.vm.provider "virtualbox" do |vb|
      vb.name = "Client-Remote"
      vb.memory = "512"
      vb.cpus = 1
    end
    remote.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/client_remote.yml"
      ansible.inventory_path = "ansible/inventory.ini"
      ansible.limit = "vm5"
      ansible.extra_vars = {
        ansible_ssh_pipelining: true
      }
    end
  end

  # VM6 - SIEM (Wazuh)
  config.vm.define "vm6" do |siem|
    siem.vm.hostname = "wazuh-server"
    siem.vm.network "private_network", ip: "192.168.56.60"
    siem.vm.provider "virtualbox" do |vb|
      vb.name = "Wazuh-SIEM"
      vb.memory = "2048" 
      vb.cpus = 2
    end
    siem.vm.provision "ansible" do |ansible|
      ansible.playbook = "ansible/siem.yml"
      ansible.inventory_path = "ansible/inventory.ini"
      ansible.limit = "vm6"
      ansible.extra_vars = {
        ansible_ssh_pipelining: true  
      }
    end
  end
end
