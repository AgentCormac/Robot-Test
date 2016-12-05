################################################################
#
# Author: 	Mark Higginbottom
#
# Date:		27/09/2016
#
#Vagrant PROJECT file to create Ansible provisioned VM for Robot Framework testing 
#
#
#################################################################
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

 
Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  
  ##robot VM
  config.vm.define "robot" do |robot|
#    robot.vm.box = "bento/centos-7.2"
    robot.vm.box = "pbarriscale/centos7-gui"
    robot.vm.hostname = 'robot'

    robot.vm.network :private_network, ip: "192.168.100.101"
    robot.vm.network :forwarded_port, guest: 22, host: 10122, id: "ssh"
    robot.vm.network :forwarded_port, guest: 7272, host: 7272, id: "robottestport"


    robot.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      v.customize ["modifyvm", :id, "--memory", 2018]
      v.customize ["modifyvm", :id, "--name", "robot"]
	  v.gui = true
    end

	#map shared directory
	robot.vm.synced_folder "shared", "/home/vagrant/shared"

	robot.vm.provision "shell", inline: <<-SHELL
		#set ownership of shared directory to vagrant
		sudo chown -R vagrant:vagrant /home/vagrant/shared
		##Install Ansible on robot node
		#sudo apt-get update -y
		sudo yum install vim -y
		sudo yum install epel-release -y
		sudo yum install ansible -y	
		sudo yum install yum-utils
		##Install Git
		sudo yum install git -y
		##Turn off ansible key checking
		export ANSIBLE_HOST_KEY_CHECKING=false
	SHELL

	##run info script
    robot.vm.provision "info", type: "shell", path: "scripts/vminfo.sh"  

	##Ansible provisioning from playbook that is on the Guest VM
	robot.vm.provision "ansible_local" do |ansible|
		ansible.verbose = "true"
		ansible.extra_vars = {servers: "robot"} #inject the name of the server we want to apply this ansible config to.
		ansible.playbook = "shared/ansible/robot-test1/site.yml"
    end
		
  end

end
