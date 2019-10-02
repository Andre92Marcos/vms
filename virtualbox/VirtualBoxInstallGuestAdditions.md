Ubuntu

Go To Devices and select Insert Guest additions CD image

Run the image (from the ui or by sudo sh /media/cdrom0/VboxGuestAdditions.run

If there is an error in the installation run:

    sudo apt install linux-headers-$(uname -r)

    sudo apt install build-essential dkms linux-headers-generic

    sudo apt install gcc make perl dkms

    sudo rcvboxadd setup

    sudo install virtualbox-guest-additions-iso

    Insert Guest additions CD image and run the image again, It should install without errors


Debian

sudo echo deb http://ftp.debian.org/debian stretch-backports main contrib > /etc/apt/sources.list.d/stretch-backports.list 

sudo apt update
	
sudo apt install linux-headers-$(uname -r)

sudo apt install build-essential dkms

sudo apt install gcc make perl

sudo apt install virtualbox-guest-dkms

Insert Guest additions CD image and run the image again, It should install without errors
