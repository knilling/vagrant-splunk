# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "centos/7"
  config.vm.network "public_network", :bridge => "eno1"
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end
  config.vm.provision "shell", inline: <<-SHELL
    yum install -y net-tools
    F=splunk.rpm
    chmod 744 "/vagrant/${F}"
    rpm -i "/vagrant/${F}"
# default credentials
cat << 'EOF' > /opt/splunk/etc/system/local/user-seed.conf
[user_info]
USERNAME = admin
PASSWORD = password
EOF
# Configure to listen for syslog
    mkdir -p /opt/splunk/etc/apps/search/local
cat << 'EOF' > /opt/splunk/etc/apps/search/local/inputs.conf
[udp://514]
connection_host = dns
sourcetype = syslog
EOF
    /opt/splunk/bin/splunk start --accept-license
    /opt/splunk/bin/splunk enable boot-start 
  SHELL
end
