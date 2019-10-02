**Print the status of all VMs**

	vagrant global-status

**Print all Vagrant commands**

	vagrant

**Stop A running Machine**

	vagrant halt machine_id

**Start a Stopped Machine**

	vagrant reload machine_id

**Show ports used by VM**

	vagrant port machine_id

**Copy files to host**

	scp -P 2200 bms-logic-db-schema-sql.zip vagrant@localhost:/vagrant
	pass:vagrant

**Force update of VM status**

	vagrant global-status --prune


**Gawk in ssh**

    vagrant ssh 21b22bf -c 'docker container ls | gawk '"'"'{for(i=1; i <=NF;i++){if ($i == \"bms-support-tool\"){print $i;}}}'"'"''
