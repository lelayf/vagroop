

vagrant box add centos-6.4-64 http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130309.box

vagrant up --provider=virtualbox

You might want to adjust memory settings in Vagrantfile and/or add more slave nodes.

Then navigate to 10.211.55.100:7180
User / pass : admin/admin

Once in Cloudera manager use vagrant/vagrant for sudoer user.

