# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"

cluster = {
  "freeipa" => {
    :ip => "192.168.172.80",
    :cpus => 2,
    :mem => 2048,
    :distro => "generic/fedora35",
    :bridgeadapter1 => "wlp0s20f3"
  },
  "rancher" => {
    :ip => "192.168.172.81",
    :cpus => 2,
    :mem => 3072,
    :distro => "generic/ubuntu2004",
    :bridgeadapter1 => "wlp0s20f3"
  },
  "master" => {
    :ip => "192.168.172.82",
    :cpus => 3,
    :mem => 5120,
    :distro => "generic/fedora35",
    :bridgeadapter1 => "wlp0s20f3"
  },
  "worker1" => {
    :ip => "192.168.172.83",
    :cpus => 3,
    :mem => 5120,
    :distro => "generic/fedora35",
    :bridgeadapter1 => "wlp0s20f3"
  },
  "worker2" => {
    :ip => "192.168.172.84",
    :cpus => 3,
    :mem => 5120,
    :distro => "generic/fedora35",
    :bridgeadapter1 => "wlp0s20f3"
  },
  "vault" => {
    :ip => "192.168.172.85",
    :cpus => 2,
    :mem => 4096,
    :distro => "generic/ubuntu2004",
    :bridgeadapter1 => "wlp0s20f3"
  }
}

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  total_hosts = cluster.length
  
  cluster.each_with_index do |(hostname, info), index|

    config.vm.define hostname do |cfg|
      cfg.vm.provider :virtualbox do |vb, override|
        config.vm.box = info[:distro]
        config.vm.box_check_update = "false"
        
        vb.name = hostname
        vb.customize [
          "modifyvm", :id,
          "--memory", info[:mem],
          "--cpus", info[:cpus],
          "--hwvirtex", "on",
          "--hostonlyadapter1", "none"
        ]

        override.vm.network :public_network, ip: "#{info[:ip]}", bridge: info[:bridgeadapter1], hostname: true
        override.vm.hostname = "#{hostname}.my.local"

        if hostname == "freeipa"
          cfg.vm.synced_folder "playbooks/", "/home/vagrant/ansible"
        end

      end

    end

  end

end