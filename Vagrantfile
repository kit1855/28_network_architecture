# -*- mode: ruby -*-
# vim: set ft=ruby :

ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

MACHINES = {
  :inetRouter => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "inetRouter",
    :net => [
      {ip: '192.168.255.1', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
    ]
  },
  :centralRouter => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "centralRouter",
    :net => [
      {ip: '192.168.255.2', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "router-net"},
      {ip: '192.168.0.1', adapter: 3, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
      {ip: '192.168.0.33', adapter: 4, netmask: "255.255.255.240", virtualbox__intnet: "hw-net"},
      {ip: '192.168.0.65', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "mgt-net"},
    ]
  },
  :centralServer => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "centralServer",
    :net => [
      {ip: '192.168.0.2', adapter: 2, netmask: "255.255.255.240", virtualbox__intnet: "dir-net"},
      {adapter: 3, auto_config: false, virtualbox__intnet: true},
      {adapter: 4, auto_config: false, virtualbox__intnet: true},
    ]
  },
  :office1Router => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "office1Router",
    :net => [
      {ip: '192.168.255.10', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office1-central"},
      {ip: '192.168.2.1', adapter: 3, netmask: "255.255.255.192", virtualbox__intnet: "dev-net"},
      {ip: '192.168.2.65', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "test-net"},
      {ip: '192.168.2.129', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "managers-net"},
      {ip: '192.168.2.193', adapter: 6, netmask: "255.255.255.192", virtualbox__intnet: "hw-net"},
    ]
  },
  :office1Server => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "office1Server",
    :net => [
      {ip: '192.168.2.130', adapter: 2, netmask: "255.255.255.192", virtualbox__intnet: "managers-net"},
    ]
  },
  :office2Router => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "office2Router",
    :net => [
      {ip: '192.168.255.6', adapter: 2, netmask: "255.255.255.252", virtualbox__intnet: "office2-central"},
      {ip: '192.168.1.1', adapter: 3, netmask: "255.255.255.128", virtualbox__intnet: "dev-net"},
      {ip: '192.168.1.129', adapter: 4, netmask: "255.255.255.192", virtualbox__intnet: "test-net"},
      {ip: '192.168.1.193', adapter: 5, netmask: "255.255.255.192", virtualbox__intnet: "hw-net"},
    ]
  },
  :office2Server => {
    :box_name => "ubuntu/jammy64",
    :vm_name => "office2Server",
    :net => [
      {ip: '192.168.1.2', adapter: 2, netmask: "255.255.255.128", virtualbox__intnet: "dev-net"},
    ]
  }
}

Vagrant.configure("2") do |config|
  MACHINES.each do |boxname, boxconfig|
    config.vm.define boxname do |box|
      box.vm.box = boxconfig[:box_name]
      box.vm.host_name = boxconfig[:vm_name]

      box.vm.provider "virtualbox" do |v|
        v.memory = 768
        v.cpus = 1
      end

      # Исправленный синтаксис для Vagrant 2.4.9
      boxconfig[:net].each do |ipconf|
        box.vm.network "private_network", **ipconf
      end

      if boxconfig.key?(:public)
        box.vm.network "public_network", boxconfig[:public]
      end

      box.vm.provision "shell", inline: <<-SHELL
        mkdir -p ~root/.ssh
        cp ~vagrant/.ssh/auth* ~root/.ssh
      SHELL

      case boxname.to_s
      when "inetRouter"
        box.vm.provision "shell", run: "always", inline: <<-SHELL
          sysctl -w net.ipv4.conf.all.forwarding=1
          sysctl -w net.ipv4.ip_forward=1
          echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf
          echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
          iptables -t nat -A POSTROUTING ! -d 192.168.0.0/16 -o eth0 -j MASQUERADE
        SHELL
      when "centralRouter"
        box.vm.provision "shell", run: "always", inline: <<-SHELL
          sysctl -w net.ipv4.conf.all.forwarding=1
          sysctl -w net.ipv4.ip_forward=1
          echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf
          echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
          # Удаляем дефолтный маршрут через eth0
          ip route del default via 10.0.2.2 2>/dev/null || true
          # Добавляем маршрут по умолчанию через inetRouter
          ip route add default via 192.168.255.1
        SHELL
      when "office1Router"
        box.vm.provision "shell", run: "always", inline: <<-SHELL
          sysctl -w net.ipv4.conf.all.forwarding=1
          sysctl -w net.ipv4.ip_forward=1
          echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf
          echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
          ip route del default via 10.0.2.2 2>/dev/null || true
          ip route add default via 192.168.255.9
        SHELL
      when "office2Router"
        box.vm.provision "shell", run: "always", inline: <<-SHELL
          sysctl -w net.ipv4.conf.all.forwarding=1
          sysctl -w net.ipv4.ip_forward=1
          echo "net.ipv4.conf.all.forwarding = 1" >> /etc/sysctl.conf
          echo "net.ipv4.ip_forward = 1" >> /etc/sysctl.conf
          ip route del default via 10.0.2.2 2>/dev/null || true
          ip route add default via 192.168.255.5
        SHELL
      when "centralServer"
        box.vm.provision "shell", run: "always", inline: <<-SHELL
          ip route del default via 10.0.2.2 2>/dev/null || true
          ip route add default via 192.168.0.1
        SHELL
      when "office1Server"
        box.vm.provision "shell", run: "always", inline: <<-SHELL
          ip route del default via 10.0.2.2 2>/dev/null || true
          ip route add default via 192.168.2.129
        SHELL
      when "office2Server"
        box.vm.provision "shell", run: "always", inline: <<-SHELL
          ip route del default via 10.0.2.2 2>/dev/null || true
          ip route add default via 192.168.1.1
        SHELL
      end
    end
  end
end
