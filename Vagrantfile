# -*- mode: ruby -*-
# vi: set ft=ruby :

vms = {
  "cicd"       => {"memory"=>"1536", "cpus"=>"1", "ip" => "10", "disk" => "40GB", "box" => "roboxes/ubuntu2004", "provision" => "provision/ansible/cicd.yaml"},
  "cicd-tools" => {"memory"=>"3072", "cpus"=>"1", "ip" => "20", "disk" => "40GB", "box" => "roboxes/ubuntu2004", "provision" => "provision/ansible/cicd-tools.yaml"},
  "k3s"        => {"memory"=>"2048", "cpus"=>"2", "ip" => "30", "disk" => "60GB", "box" => "roboxes/ubuntu2004", "provision" => "provision/ansible/k3s.yaml"},
  "gitlab-ci"  => {"memory"=>"3072", "cpus"=>"2", "ip" => "40", "disk" => "80GB", "box" => "roboxes/ubuntu2004", "provision" => "provision/ansible/gitlab-ci.yaml"}
}

Vagrant.configure("2") do |config|
  # Verifica se o plugin do LIBVIRT está instalado
  required_plugins = %w(vagrant-libvirt)
  _retry = false
  required_plugins.each do |plugin|
    unless Vagrant.has_plugin? plugin
      system "vagrant plugin install #{plugin}"
      _retry = true
    end
  end

  if _retry
    puts "Plugins instalados. Execute novamente: vagrant up"
    exit
  end

  vms.each do |name, conf|
    config.vm.define "#{name}" do |k|
      k.vm.box = conf["box"]
      k.vm.hostname = name

      # Disco virtual
      k.vm.disk :disk, size: conf["disk"], primary: true

      # Rede privada
      k.vm.network "private_network", ip: "192.168.88.#{conf['ip']}"

      # Configurações do provedor LIBVIRT
      k.vm.provider "libvirt" do |libvirt|
        libvirt.memory = conf["memory"]
        libvirt.cpus = conf["cpus"]
        libvirt.video_vram = 16
        libvirt.nested = true
        libvirt.storage_pool_name = "default"

      end

      # Provisionamento via Ansible Local
      k.vm.provision "ansible_local" do |ansible|
        ansible.playbook = conf["provision"]
        ansible.compatibility_mode = "2.0"
      end
    end
  end
end
