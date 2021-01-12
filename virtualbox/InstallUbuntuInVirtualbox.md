1st) Download a ubunto .iso

2nd) Create a virtualbox

3rd) Go to settings of the virtualbox created -> Storage -> Empty Controller IDE -> and click to 
add a new disk -> choose the ubunto .iso

4th) Install ubunto as normally

**You may need to sign kernel modules if UEFI Secure Boot is enabled**

	sudo apt install mokutil
	sudo -i
	mkdir /root/signed-modules
	cd /root/signed-modules
	openssl req -new -x509 -newkey rsa:2048 -keyout MOK.priv -outform DER -out MOK.der -nodes -days 36500 -subj "/CN=VirtualBox/"
	chmod 600 MOK.priv
	mokutil --import MOK.der
	This command will ask for a password

	Reboot the system and when a blue screen appears select Enroll MOK --> Continue -->put the previous password and confirm it

	Create and run this script: vi sign-virtual-box

	'''
	#!/bin/bash
	for modfile in $(dirname $(modinfo -n vboxdrv))/*.ko;do
		echo "Signing $modfile
		/usr/src/kernels/$(uname -r)/scripts/sign-file sha256 /root/signed-modules/MOK.priv /root/signed-modules/MOK.der "$modfile"
	done

	'''

	chmod 700 sign-virtual-box
	./sign-virtual-box

	modprobe vboxdrv

	https://stackoverflow.com/questions/61248315/sign-virtual-box-modules-vboxdrv-vboxnetflt-vboxnetadp-vboxpci-centos-8
