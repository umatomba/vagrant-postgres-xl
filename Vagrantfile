# -*- mode: ruby -*-
# vi: set ft=ruby : 

CLUSTER_SIZE = 3
FROM_IP = "172.16.0.101"
HOSTNAME_PREF = 'hc'

def get_nodes (count, from_ip, hostname_pref)
    nodes = []
    ip_arr = from_ip.split('.')
    first_ip_part = "#{ip_arr[0]}.#{ip_arr[1]}.#{ip_arr[2]}"
    count.times do |i|
        hostname = "%s%01d" % [hostname_pref, (i+1)]
        nodes.push([i+1, hostname, "#{first_ip_part}.#{ip_arr.last.to_i+i}"])
    end
    nodes
end

def provision_node(hostaddr, node_addresses)
    setup = <<-SCRIPT
    SCRIPT
end

def start_cluster()
    ret = <<-SCRIPT
    SCRIPT
end

def attachnode()
    ret = <<-SCRIPT
    SCRIPT
end

Vagrant.configure("2") do |config|
    config.vm.box = "chef/centos-6.5"
    config.ssh.username = "vagrant"
    config.ssh.password = "vagrant"
    config.ssh.shell = "bash -c 'BASH_ENV=/etc/profile exec bash'"

    if Vagrant.has_plugin?("vagrant-cachier")
        config.cache.scope = :box
        config.cache.enable :apt
    end

    cluster_nodes = get_nodes(CLUSTER_SIZE, FROM_IP, HOSTNAME_PREF)
    cluster_ips = []
    cluster_nodes.each {|a,b,c| cluster_ips.push(c)}

    cluster_nodes.each do |number, hostname, hostaddr|
        config.vm.define hostname do |box|
            box.vm.hostname = "#{hostname}"
            box.vm.network :private_network, ip: "#{hostaddr}", :netmask => "255.255.0.0"
            box.vm.provision :shell, :inline => provision_node(hostaddr, cluster_ips)
            if number == 1
                #box.vm.provision :shell, :inline => start_cluster()
            else
                #box.vm.provision :shell, :inline => attachnode()
            end
        end
    end
end
