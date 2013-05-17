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

Vagrant.configure("2") do |config|

  config.vm.define :master do |master|
    master.vm.box = "centos-6.4-64"
    master.vm.provider "vmware_fusion" do |v|
      v.vmx["memsize"]  = "4096"
    end
    master.vm.provider :virtualbox do |v|
      v.name = "vm-cluster-node1"
      v.customize ["modifyvm", :id, "--memory", "4096"]
    end
    master.vm.network :private_network, ip: "10.211.55.100"
    master.vm.hostname = "vm-cluster-node1"
    master.vm.provision :shell, :inline => $master_script
  end

end

