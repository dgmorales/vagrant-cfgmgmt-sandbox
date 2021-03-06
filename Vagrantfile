# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"
  config.vm.provision "shell", path: "provision/cm-install.sh"
  # set some names <=> ips inside all machines
  config.vm.provision :hosts do |h|
      # default servers for puppet agents and salt minions
      h.add_host '192.168.100.5', ['puppet', 'salt']
  end
  #TODO: this + below runs puppet twice
  config.vm.provision "puppet" do |puppet|
    puppet.environment_path = "puppet/envs"
  end
  # we need to this so vagrant generates the ansible inventory for us
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "ansible/nothing.yml"
    ansible.groups = {
      "rabbits" => ["m[1:2]"]
    }
  end

  config.vm.define :cm do |cm|
    cm.vm.provider "virtualbox" do |vb|
       vb.memory = "2048"
    end
    cm.vm.hostname = "cmserver.local"
    cm.vm.network "private_network", ip: "192.168.100.5", virtualbox__intnet: "cmnet"
    cm.vm.network "forwarded_port", guest: 80, host: 9080
    cm.vm.network "forwarded_port", guest: 4440, host: 4440
    #cm.vm.provision "docker" do |d|
    #  d.run "redisio", daemonize: true, image: "redis"
    #  d.run "mongodb", daemonize: true, image: "mongo", args: "-p 127.0.0.1:27017:27017"
    #  d.run "semaphore", daemonize: true, image: "castawaylabs/semaphore", args: "--link redisio:redis --link mongodb:mongo -e MONGODB_URL='mongodb://mongo/semaphore' -e REDIS_HOST='redis' -p 80:80"
    #  d.run "rundeck", daemonize: true, image: "jordan/rundeck", args: "-p 4440:4440 -e SERVER_URL=http://127.0.0.1:4440 -v /opt/rundeck-plugins:/opt/rundeck-plugins"
    #end
  end

  # specify all these settings only once.
  config.vm.define :m1 do |m1|
    #m1.vm.provision "puppet" do |puppet|
    #  puppet.environment_path = "puppet/envs"
    #  puppet.manifests_path = "puppet/envs/production/manifests"
    #  puppet.manifest_file = "full.pp"
    #  puppet.hiera_config_path = "puppet/envs/production/hiera.yaml"
    #end
    m1.vm.hostname = "m1.local"
    m1.vm.network "private_network", ip: "192.168.100.11", virtualbox__intnet: "cmnet"
    m1.vm.network "forwarded_port", guest: 15672, host: 15672  # rabbitmq mgmt
  end
  config.vm.define :m2 do |m2|
    #m2.vm.provision "puppet" do |puppet|
    #  puppet.environment_path = "puppet/envs"
    #  puppet.manifests_path = "puppet/envs/production/manifests"
    #  puppet.manifest_file = "full.pp"
    #  puppet.hiera_config_path = "puppet/envs/production/hiera.yaml"
    #end
    m2.vm.hostname = "m2.local"
    m2.vm.network "private_network", ip: "192.168.100.12", virtualbox__intnet: "cmnet"
    m2.vm.network "forwarded_port", guest: 15672, host: 25672  # rabbitmq mgmt
  end
  config.vm.define :m3 do |m3|
    m3.vm.hostname = "m3.local"
    m3.vm.network "private_network", ip: "192.168.100.13", virtualbox__intnet: "cmnet"
  end

  config.vm.provider "virtualbox" do |vb|
     # Customize the amount of memory on the VM:
     vb.memory = "512"
  end
end
