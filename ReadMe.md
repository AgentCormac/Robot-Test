Robot-Test
============

This is a Vagrant environment for testing/learning about Robot Framework when you are handicapped by having a Windows machine. ;0)

The Vagrant script will spin up a CentOS 7 gui machine and install Robot Framework WebDemo:

**NOTE:** This is not the way to write an Ansible Playbook. It is just a quick and dirty script to get up and running quickly.

Instructions
------------

Vagrant up
* Wait for provisioning to complete
* Use GUI to login - vagrant/vagrant
* Open a terminal window
```
cd webdemo
python demoapp/server.py &
robot login-tests/
```
**NOTE:** Tests can be disrupted by Firefox setup dialogues, which can happen after an update/install.
**NOTE:** Running a gui in a vm is not a good idea. May need to increase the wait time and/or memory allocated to the VM for the vm to enable the browser to respond in time for the framework.
```
${DELAY}          0 in login_tests/resource.robot
```

Pre-requisites
--------------

* Virtual Box
* Cygwin
* Vagrant
* Vagrant plugin vagrant-vbguest

The Vagrantfile will spin up the VMs and install Ansible, ssh-keys etc. The ansible script can be found in the **shared** directory.




Enjoy!
