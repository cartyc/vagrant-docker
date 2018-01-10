# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
 
	config.vm.box = "centos/7"
	## To enable synced folders install guest additions and uncomment the synced folder line
	# Install Guest additions https://www.if-not-true-then-false.com/2010/install-virtualbox-guest-additions-on-fedora-centos-red-hat-rhel/
	# config.vm.synced_folder ".", "/code"
	config.vm.network "forwarded_port", guest: 80, host: 8080


	# If this box will be used on the a network, add some certs to the vm's
	# cert CA store.  This is needed to run git and composer (and probably others)
	config.vm.provision "file", source: "cacert.pem", destination: "/tmp/CESCA001.pem"
	script = StringIO.new
	script << "update-ca-trust force-enable\n"
	script << "cp -f /tmp/*.pem /etc/pki/ca-trust/source/anchors/\n"
	script << "update-ca-trust extract\n"
	script << "exit 0\n"
	config.vm.provision "shell", inline: script.string

	config.vm.provision "shell", inline: "yum install -y yum-utils \
		  device-mapper-persistent-data \
		  lvm2 && \
		yum-config-manager \
		    --add-repo \
		    https://download.docker.com/linux/centos/docker-ce.repo && \
		yum install -y docker-ce && \
		sudo usermod -aG docker vagrant && \
		systemctl start docker && \
		systemctl enable docker" 
end
