# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
$script = <<SCRIPT

echo 'deb http://archive.ubuntu.com/ubuntu precise main universe' > /etc/apt/sources.list && \
echo 'deb http://archive.ubuntu.com/ubuntu precise-updates universe' >> /etc/apt/sources.list && \
apt-get update


#Prevent daemon start during install
 echo '#!/bin/sh\nexit 101' > /usr/sbin/policy-rc.d && \
chmod +x /usr/sbin/policy-rc.d

 DEBIAN_FRONTEND=noninteractive apt-get clean
 DEBIAN_FRONTEND=noninteractive apt-get autoclean
 DEBIAN_FRONTEND=noninteractive apt-get update
 DEBIAN_FRONTEND=noninteractive apt-get upgrade

#Supervisord
 DEBIAN_FRONTEND=noninteractive apt-get install -y supervisor && \
mkdir -p /var/log/supervisor
/usr/bin/supervisord -n

#Utilities
#RUN DEBIAN_FRONTEND=noninteractive apt-get install -y vim less nano ntp net-tools inetutils-ping curl git telnet

#Install Oracle Java 7
echo 'deb http://ppa.launchpad.net/webupd8team/java/ubuntu precise main' > /etc/apt/sources.list.d/java.list && \
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys EEA14886 && \
apt-get update && \
echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && \
DEBIAN_FRONTEND=noninteractive apt-get install -y oracle-java7-installer
#ElasticSearch
 wget https://download.elasticsearch.org/elasticsearch/elasticsearch/elasticsearch-1.5.0.tar.gz && \
tar zxvf elasticsearch-*.tar.gz && \
rm elasticsearch-*.tar.gz && \
mv elasticsearch-* elasticsearch && \
elasticsearch/bin/plugin -install mobz/elasticsearch-head
#Kibana
 wget https://download.elasticsearch.org/kibana/kibana/kibana-4.0.1-linux-x64.tar.gz && \
tar zxvf kibana-*.tar.gz && \
rm kibana-*.tar.gz && \
mv kibana-* kibana
#Logstash
 wget https://download.elasticsearch.org/logstash/logstash/logstash-1.4.3.tar.gz && \
tar zxvf logstash-*.tar.gz && \
rm logstash-*.tar.gz && \
mv logstash-* logstash

SCRIPT 


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "parallels/ubuntu-13.10"

  config.vm.network "public_network"

  config.vm.synced_folder ".", "/workspace"
  
  config.vm.provision "docker"

  end
end
