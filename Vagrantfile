# -*- mode: ruby -*-
# vi: set ft=ruby :

## --- config -------- ##

password = ENV['ST2HANDSON_PASSWORD'] || 'changeme'

# mgmt: 172.16.0.0/24

mgmt_st2   = '172.16.0.10'
mgmt_sensu = '172.16.0.12'
mgmt_r1    = '172.16.0.101'
mgmt_r2    = '172.16.0.102'
monitor_sensu_ipv4 = '10.1.234.12'
monitor_r1_ipv4    = '10.1.234.101'
monitor_sensu_ipv6 = '2001:db8:1:234::12'
monitor_r1_ipv6    = '2001:db8:1:234::101'

## ------------------- ##


st2user = 'st2admin'
st2pass = password
sensu_user = 'admin'
sensu_pass = password


Vagrant.configure(2) do |config|

  config.vm.define :st2 do |st2|
    st2.vm.box = "bento/ubuntu-16.04"
    st2.vm.hostname = "st2"

    st2.vm.network :forwarded_port,
      guest: 443,
      host: 8443

    st2.vm.network :private_network,
      ip: mgmt_st2,
      virtualbox__intnet: 'st2handson_mgmt'

    st2.vm.provider :virtualbox do |v|
      v.cpus = 2
      v.memory = 2048
    end

    # setup st2
    st2.vm.provision :ansible_local,
      install: true,
      playbook: "playbook/st2/stackstorm.yml",
      raw_arguments: ["-e \"st2_auth_username=#{st2user}\"",
                      "-e \"st2_auth_password=#{st2pass}\""]
  end

  config.vm.define "sensu" do |sensu|
    sensu.vm.box = "bento/ubuntu-16.04"
    sensu.vm.hostname = 'sensu'

    sensu.vm.network :forwarded_port,
      guest: 3000,
      host: 3000

    sensu.vm.network :private_network,
      ip: mgmt_sensu,
      virtualbox__intnet: "st2handson_mgmt"

    sensu.vm.network "private_network",
      ip: monitor_sensu_ipv4,
      virtualbox__intnet: "st2handson_r1-sensu"

    sensu.vm.provider :virtualbox do |v|
      v.memory = 512
      v.cpus = 1
    end

    sensu.vm.provision :ansible_local,
      install: true,
      playbook: "playbook/sensu/sensu.yml",
      raw_arguments: ["-e \"st2_username=#{st2user}\"",
                      "-e \"st2_password=#{st2pass}\"",
                      "-e \"st2_hostname=#{mgmt_st2}\"",
                      "-e \"sensu_username=#{sensu_user}\"",
                      "-e \"sensu_password=#{sensu_pass}\"",
                      "-e \"monitor_ipv4=#{monitor_sensu_ipv4}\"",
                      "-e \"monitor_ipv6=#{monitor_sensu_ipv6}\"",
                      "-e \"monitor_nw_ipv4=10.0.0.0/8\"",
                      "-e \"monitor_nw_ipv6=2001:db8::/32\"",
                      "-e \"monitor_gw_ipv4=#{monitor_r1_ipv4}\"",
                      "-e \"monitor_gw_ipv6=#{monitor_r1_ipv6}\"",
                      "-e \"monitor_target_ipv4=10.2.0.102\"",
                      "-e \"monitor_target_ipv6=2001:db8:2::102\""]

  end

  # r1
  config.vm.define :r1 do |r1|
    r1.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    r1.vm.hostname = "r1"

    r1.vm.network "private_network",
      ip: mgmt_r1,
      virtualbox__intnet: "st2handson_mgmt"

    r1.vm.network "private_network",
      ip: "10.1.12.101",
      virtualbox__intnet: "st2handson_r1-r2"

    r1.vm.network "private_network",
      ip: monitor_r1_ipv4,
      virtualbox__intnet: "st2handson_r1-sensu"

    r1.vm.provider "virtualbox" do |v|
      v.check_guest_additions = false
      v.memory = 1024
      v.customize ["modifyvm", :id,
                    "--nicpromisc2", "deny",
                    "--nicpromisc3", "deny"]
    end

    r1.vm.provision "file",
      source: File.expand_path("../config-vsrx/setup.cli", __FILE__),
      destination: "/cf/root/setup.cli"

    r1.vm.provision "file",
      source: File.expand_path("../config-vsrx/merge_after_reboot.cli", __FILE__),
      destination: "/cf/root/merge_after_reboot.cli"

    r1.vm.provision "file",
      source: File.expand_path("../config-vsrx/r1.patch",  __FILE__),
      destination: "/cf/root/setup.patch"

    r1.vm.provision :host_shell,
      inline: 'vagrant ssh r1 -c "/usr/sbin/cli -f /cf/root/setup.cli"'

    r1.vm.provision :host_shell, run: :always,
      inline: 'vagrant ssh r1 -c "/usr/sbin/cli -f /cf/root/merge_after_reboot.cli"'

  end

  # r2
  config.vm.define :r2 do |r2|
    r2.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"
    r2.vm.hostname = "r2"

    r2.vm.network "private_network",
      ip: mgmt_r2,
      virtualbox__intnet: "st2handson_mgmt"

    r2.vm.network "private_network",
      ip: "10.1.12.102",
      virtualbox__intnet: "st2handson_r1-r2"

    r2.vm.provider :virtualbox do |v|
      v.check_guest_additions = false
      v.memory = 1024
      v.customize ["modifyvm", :id,
                    "--nicpromisc2", "deny",
                    "--nicpromisc3", "deny"]
    end

    r2.vm.provision :file,
      source: File.expand_path("../config-vsrx/setup.cli", __FILE__),
      destination: "/cf/root/setup.cli"

    r2.vm.provision "file",
      source: File.expand_path("../config-vsrx/merge_after_reboot.cli", __FILE__),
      destination: "/cf/root/merge_after_reboot.cli"

    r2.vm.provision :file,
      source: File.expand_path("../config-vsrx/r2.patch",  __FILE__),
      destination: "/cf/root/setup.patch"

    r2.vm.provision :host_shell,
      inline: 'vagrant ssh r2 -c "/usr/sbin/cli -f /cf/root/setup.cli"'

    r2.vm.provision :host_shell, run: :always,
      inline: 'vagrant ssh r2 -c "/usr/sbin/cli -f /cf/root/merge_after_reboot.cli"'

  end
end
