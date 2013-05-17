# -*- mode: ruby -*-
# vi: set ft=ruby :

$master_script = <<SCRIPT
#!/bin/bash
cat > /etc/hosts <<EOF
127.0.0.1       localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

10.211.55.100   vm-cluster-node1
10.211.55.101   vm-cluster-node2
10.211.55.105   vm-cluster-client
EOF

yum install curl -y
cat > /etc/yum.repos.d/cloudera-cm4.repo <<EOF
[cloudera-manager]
# Packages for Cloudera Manager, Version 4, on RedHat or CentOS 6 x86_64
name=Cloudera Manager
baseurl=http://archive.cloudera.com/cm4/redhat/6/x86_64/cm/4/
gpgkey = http://archive.cloudera.com/cm4/redhat/6/x86_64/cm/RPM-GPG-KEY-cloudera    
gpgcheck = 1
EOF
rpm --import http://archive.cloudera.com/cm4/redhat/6/x86_64/cm/RPM-GPG-KEY-cloudera
yum update
yum -q -y install jdk cloudera-manager-server-db cloudera-manager-server cloudera-manager-daemons
service iptables stop
chkconfig iptables off
service cloudera-scm-server-db initdb
service cloudera-scm-server-db start
service cloudera-scm-server start
SCRIPT

$slave_script = <<SCRIPT
cat > /etc/hosts <<EOF
127.0.0.1       localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

10.211.55.100   vm-cluster-node1
10.211.55.101   vm-cluster-node2
10.211.55.105   vm-cluster-client

service iptables stop
chkconfig iptables off

EOF
SCRIPT

$client_script = <<SCRIPT
cat > /etc/hosts <<EOF
127.0.0.1       localhost

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters

10.211.55.100   vm-cluster-node1
10.211.55.101   vm-cluster-node2
10.211.55.105   vm-cluster-client
EOF
SCRIPT

Vagrant.configure("2") do |config|

  config.vm.define :master do |master|
    master.vm.box = "centos-6.4-64"
    master.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = "1536"
    end
    master.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node1"
      v.customize ["modifyvm", :id, "--memory", "1536"]
    end
    master.vm.network :private_network, ip: "10.211.55.100"
    master.vm.hostname = "vm-cluster-node1"
    master.vm.provision :shell, :inline => $master_script
    #master.vm.network :forwarded_port, guest: 7180, host: 7180
  end

  config.vm.define :slave1 do |slave1|
    slave1.vm.box = "centos-6.4-64"
    slave1.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = "1536"
    end
    slave1.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node2"
      v.customize ["modifyvm", :id, "--memory", "1536"]
    end
    slave1.vm.network :private_network, ip: "10.211.55.101"
    slave1.vm.hostname = "vm-cluster-node2"
    slave1.vm.provision :shell, :inline => $slave_script
  end

  config.vm.define :client do |client|
    client.vm.box = "centos-6.4-64"
    client.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = "1536"
    end
    client.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-client"
      v.customize ["modifyvm", :id, "--memory", "1536"]
    end
    client.vm.network :private_network, ip: "10.211.55.105"
    client.vm.hostname = "vm-cluster-client"
    client.vm.provision :shell, :inline => $client_script
  end

end

