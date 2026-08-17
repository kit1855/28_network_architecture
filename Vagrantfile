# -*- mode: ruby -*-
# vim: set ft=ruby :

# Указываем зеркало для скачивания образов
ENV['VAGRANT_SERVER_URL'] = 'https://vagrant.elab.pro'

Vagrant.configure("2") do |config|

  # ============================================
  # 1. inetRouter
  # ============================================
  config.vm.define "inetRouter" do |inet|
    inet.vm.box = "ubuntu/jammy64"
    inet.vm.hostname = "inetRouter"
    inet.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end
  end

  # ============================================
  # 2. centralRouter
  # ============================================
  config.vm.define "centralRouter" do |central|
    central.vm.box = "ubuntu/jammy64"
    central.vm.hostname = "centralRouter"
    central.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end
  end

  # ============================================
  # 3. centralServer
  # ============================================
  config.vm.define "centralServer" do |srv|
    srv.vm.box = "ubuntu/jammy64"
    srv.vm.hostname = "centralServer"
    srv.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end
  end

  # ============================================
  # 4. office1Router
  # ============================================
  config.vm.define "office1Router" do |office1|
    office1.vm.box = "ubuntu/jammy64"
    office1.vm.hostname = "office1Router"
    office1.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end
  end

  # ============================================
  # 5. office1Server
  # ============================================
  config.vm.define "office1Server" do |srv|
    srv.vm.box = "ubuntu/jammy64"
    srv.vm.hostname = "office1Server"
    srv.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end
  end

  # ============================================
  # 6. office2Router
  # ============================================
  config.vm.define "office2Router" do |office2|
    office2.vm.box = "ubuntu/jammy64"
    office2.vm.hostname = "office2Router"
    office2.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end
  end

  # ============================================
  # 7. office2Server
  # ============================================
  config.vm.define "office2Server" do |srv|
    srv.vm.box = "ubuntu/jammy64"
    srv.vm.hostname = "office2Server"
    srv.vm.provider "virtualbox" do |v|
      v.memory = 512
      v.cpus = 1
    end
  end

end
