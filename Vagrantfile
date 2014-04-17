# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/precise64"
  #config.vm.box_url = "http://files.vagrantup.com/precise64.box"

  config.vm.network :forwarded_port, guest: 80, host: 2280
  config.vm.network :forwarded_port, guest: 2003, host: 2003
  config.vm.network :forwarded_port, guest: 8125, host: 8125, :protocol => 'udp'
  config.vm.network :forwarded_port, guest: 8126, host: 8126, :protocol => 'tcp'

  # config.vm.network :private_network, ip: "192.168.33.10"
  config.vm.network :public_network

  config.vm.provider :virtualbox do |vb|
    # Don't boot with headless mode
    # vb.gui = true

    # Use VBoxManage to customize the VM. For example to change memory:
    vb.customize ["modifyvm", :id, "--memory", "1024"]
  end

  # You need to ensure you have the right version of Chef installed on the box
    # vagrant plugin install vagrant-omnibus
  config.omnibus.chef_version = "11.6.0"

  # in addition to the very basics, we also have vbguest installed
  # vagrant plugin install vagrant-vbguest
  config.vbguest.auto_update = true

  # Chef Solo
  config.vm.provision :chef_solo do |chef|
    chef.cookbooks_path = "/Users/tom/chef-repo/cookbooks"
    chef.add_recipe "apt"
    chef.add_recipe "graphite"
    chef.add_recipe "statsd"
    chef.add_recipe "vim"

    chef.json = {
      :graphite => {
        :version => '0.9.12',
        :python_version => '2.7',
        :password => "********",
        :carbon => {
          :line_receiver_interface => '0.0.0.0'
        },
        :whisper => {
        },
        :graphite_web => {
        }
      },
      :statsd => {
        :graphite => {
          :global_prefix => 'statsd'
        }
      }
    }
  end
end
