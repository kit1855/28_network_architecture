# -*- mode: ruby -*-
# vim: set ft=ruby :

# Указываем зеркало для скачивания образов
ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|

# ============================================
  # 1. inetRouter (AlmaLinux 9)
  # ============================================
  config.vm.define "inetRouter" do |inet|
    inet.vm.box = "almalinux/9"
    inet.vm.hostname = "inetRouter"
    inet.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
    end
    inet.vm.network "private_network", ["192.168.255.1", 2, "255.255.255.252", "router-net"]
    inet.vm.network "private_network", ["192.168.50.10", 3, "255.255.255.0"]
  end

  # ============================================
  # 2. centralRouter
  # ============================================
  config.vm.define "centralRouter" do |central|
    central.vm.box = "almalinux/9"
    central.vm.hostname = "centralRouter"
    central.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
    end
    central.vm.network "private_network", ["192.168.255.2", 2, "255.255.255.252", "router-net"]
    central.vm.network "private_network", ["192.168.51.10", 3, "255.255.255.0"]
    central.vm.network "private_network", ["192.168.255.2", 4, "255.255.255.252", "directors-net"]
    central.vm.network "private_network", ["192.168.255.2", 5, "255.255.255.252", "office1Router-net"]
    central.vm.network "private_network", ["192.168.255.2", 6, "255.255.255.252", "office2Router-net"]
    central.vm.network "private_network", ["192.168.255.2", 7, "255.255.255.252", "hardware-net"]
    central.vm.network "private_network", ["192.168.255.2", 8, "255.255.255.252", "wifi-net"]
  end

  # ============================================
  # 3. centralServer
  # ============================================
  config.vm.define "centralServer" do |srv|
    srv.vm.box = "ubuntu/jammy64"
    srv.vm.hostname = "centralServer"
    srv.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
    end
  end

  # ============================================
  # 4. office1Router
  # ============================================
  config.vm.define "office1Router" do |office1|
    office1.vm.box = "ubuntu/jammy64"
    office1.vm.hostname = "office1Router"
    office1.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
    end
  end

  # ============================================
  # 5. office1Server
  # ============================================
  config.vm.define "office1Server" do |srv|
    srv.vm.box = "ubuntu/jammy64"
    srv.vm.hostname = "office1Server"
    srv.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
    end
  end

  # ============================================
  # 6. office2Router
  # ============================================
  config.vm.define "office2Router" do |office2|
    office2.vm.box = "ubuntu/jammy64"
    office2.vm.hostname = "office2Router"
    office2.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
    end
  end

  # ============================================
  # 7. office2Server
  # ============================================
  config.vm.define "office2Server" do |srv|
    srv.vm.box = "ubuntu/jammy64"
    srv.vm.hostname = "office2Server"
    srv.vm.provider "virtualbox" do |v|
      v.memory = 2048
      v.cpus = 2
    end
  end

end
