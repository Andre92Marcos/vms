Go to the settings of the VM -> Network


Choose the enabled adapter (such as NAT)

Click advanced

Click Port Forwarding


Host port: Port of the machine running the VM
Guest port: Port of the VM

Example:

	Guest Port 22
	Host Port 1523

Then if your VM is running a ssh server on the port 22, you can now connect to it through your machine by creating a ssh connection to your port 1523:

	ssh -p 1523 root@localhost



