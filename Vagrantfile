# -*- mode: ruby -*-
# vim: set ft=ruby :

# Указываем зеркало для скачивания образов
ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|

  # ============================================
  # 1. pxeser (Ubuntu 22.04)
  # ============================================
  config.vm.define "pxeser" do |pxeser|
    pxeser.vm.box = "ubuntu/jammy64"
    pxeser.vm.box_version = "1.0.0"
    pxeser.vm.hostname = "pxeser"

    pxeser.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    end

    # Проброс порта веб-сервера (для проверки с хоста)
    pxeser.vm.network "forwarded_port", guest: 80, host: 8080

    # Внутренняя сеть PXE (та же, что у клиента)
    pxeser.vm.network "private_network", ip: "10.0.0.20", adapter: 2, netmask: "255.255.255.0", virtualbox__intnet: "pxenet"

    # Дополнительная сеть (для доступа к хосту / интернету)
    pxeser.vm.network "private_network", ip: "192.168.56.10", adapter: 3, netmask: "255.255.255.0"

    pxeser.vm.provision "shell",
      run: "always",
      inline: <<-SHELL
        echo "pxeser is up"
        sudo ufw allow 22/tcp       # открываю порт для SSH
        sudo ufw allow 67/udp       # открываю порт для DHCP сервера
        sudo ufw allow 69/udp       # открываю порт для TFTP
        sudo ufw allow 80/tcp       # открываю порт для apache2
        sudo ufw --force enable
        sudo apt update
        sudo apt install -y dnsmasq apache2 syslinux
        sudo tee /etc/dnsmasq.d/pxe.conf > /dev/null <<'EOF'
        interface=enp0s8
        bind-interfaces
        port=0
        dhcp-range=enp0s8,10.0.0.100,10.0.0.120,255.255.255.0,12h
        dhcp-boot=pxelinux.0
        enable-tftp
        tftp-root=/srv/tftp/amd64
        EOF
        sudo mkdir -p /srv/tftp/amd64
        sudo systemctl restart dnsmasq
      SHELL
  end

  # ============================================
  # 2. pxecli (Ubuntu 22.04, загрузка по сети)
  # ============================================
  config.vm.define "pxecli" do |pxecli|
    pxecli.vm.box = "ubuntu/jammy64"
    pxecli.vm.box_version = "1.0.0"
    pxecli.vm.hostname = "pxecli"
    pxecli.vm.disk :disk, size: "50GB", primary: true
    pxecli.vm.provider "virtualbox" do |v|
      v.memory = 8192
      v.cpus = 2
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]

      # Порядок загрузки: сначала сеть (PXE), затем диск, остальное выключено
      v.customize ["modifyvm", :id, "--boot1", "net"]
      v.customize ["modifyvm", :id, "--boot2", "disk"]
      v.customize ["modifyvm", :id, "--boot3", "none"]
      v.customize ["modifyvm", :id, "--boot4", "none"]

      # ВАЖНО: привязываем первый сетевой адаптер к внутренней сети pxenet
      v.customize ["modifyvm", :id, "--nic1", "intnet"]
      v.customize ["modifyvm", :id, "--intnet1", "pxenet"]

      # Второй адаптер — NAT (для интернета после установки)
      v.customize ["modifyvm", :id, "--nic2", "nat"]
    end

    # Внутренняя сеть PXE — та же, что у сервера
    pxecli.vm.network "private_network", ip: "10.0.0.21", adapter: 3, netmask: "255.255.255.0", virtualbox__intnet: "pxenet"

  end

end
