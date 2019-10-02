# -*- mode: ruby -*-
# vi: set ft=ruby :

# Configuration
VC2_VERSION = "1.0-SNAPSHOT"

Vagrant.require_version ">= 1.6.2"

@vagrant_box_win_name = "windows-2016-sql2017"
@vagrant_box_win_url = "http://172.28.23.224/binaries/vagrant/boxes/windows_2016_server_sql2017_virtualbox.box"

@vagrant_box_red_hat_name = "centos7"
@vagrant_box_red_hat_url = "http://172.28.23.224/binaries/vagrant/boxes/centos-7-x64-virtualbox-v2.box"


Vagrant.configure("2") do |config|

	config.vm.provider :virtualbox do |v, override|
		v.gui = false
		v.customize ["setextradata", "global", "GUI/SuppressMessages", "all" ]
	end
	config.ssh.insert_key = false


	config.vm.define "vcdata-l1" do |win|

		win.vm.box = @vagrant_box_win_name
		win.vm.box_url = @vagrant_box_win_url

		win.vm.provider :virtualbox do |v, override|
			v.name = "vcdata-l1"
			v.gui = false
			v.memory = 4096
			v.cpus = 2
			v.customize ["modifyvm", :id, "--cpuexecutioncap", 90]
			v.customize ["modifyvm", :id, "--vram", 64]
			v.customize ["modifyvm", :id, "--audio", "none"]
			v.customize ["modifyvm", :id, "--clipboard", "bidirectional"]
			v.customize ["setextradata", "global", "GUI/SuppressMessages", "all" ]
		end

		win.vm.communicator = "winrm"

		# Admin user name and password
		win.winrm.username = "Administrator"
		win.winrm.password = "vagrant"

		win.vm.guest = :windows
		win.windows.halt_timeout = 15

		win.vm.network :forwarded_port, guest: 22, host: 2200, id: "ssh"
		win.vm.network :forwarded_port, guest: 1433, host: 1433, id: "sqlserver", auto_correct: true
		win.vm.network :forwarded_port, guest: 3389, host: 3389, id: "rdp", auto_correct: true
		win.vm.network :forwarded_port, guest: 8080, host: 55000, id: "vc2ui", auto_correct: true

		win.vm.network "private_network", ip: "169.254.156.232"

		win.vm.provision "shell", path: "scripts/config.ps1", args:[VC2_VERSION]

	end


	config.vm.define "plugnplay-l1" do |vpp|

		vpp.vm.box = @vagrant_box_red_hat_name
		vpp.vm.box_url = @vagrant_box_red_hat_url

		vpp.vm.provider 'virtualbox' do |v|
			v.name = "plugnplay-l1"
			v.gui = false
			v.memory = 4096
			v.cpus = 2
	
			v.customize ["modifyvm", :id, "--cpuexecutioncap", 90]
			v.customize ["modifyvm", :id, "--vram", 64]
			v.customize ["modifyvm", :id, "--audio", "none"]
			v.customize ["modifyvm", :id, "--cableconnected1", "on"]   # To workaround a VBox 5.0 bug where NAT has cableconnectd=off
		end

		vpp.vm.network :forwarded_port, guest: 22, host: 2222, id: "ssh"

		vpp.vm.network "private_network", ip: "169.254.156.233"
		vpp.vm.hostname = "plugnplay-l1.inseincvirtuals.com"

		vpp.vm.provision "shell", path: "scripts/config-vpp-node.sh"

	end

end