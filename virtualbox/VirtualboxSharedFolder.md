**Creating a shared folder**

Create a folder on the Host computer (ubuntu) that you would like to share, for example ~/share
Boot the Guest operating system in VirtualBox.
shaSelect Devices -> Shared Folders...
Choose the 'Add' button.
Select ~/share
Optionally select the 'Make permanent' option

## Linux

With a shared folder named share, as above, the folder can be mounted as the directory ~/host with the command

    sudo mount -t vboxsf -o uid=$UID,gid=$(id -g) share ~/host

## Windows

On the Windows Guest, run

    net use x: \\vboxsvr\share
