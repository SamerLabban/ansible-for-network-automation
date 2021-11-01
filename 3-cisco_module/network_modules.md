## Network Modules - Cisco:

### Ansible ad-hoc with Cisco Devices:

Use ansible ad-hoc command to execute some cisco show command:
- Go to your virtual environment "ansible-2.9.0"
- cd ~/tmp
- ansible -i hosts ios -m raw -a "show version" <br/>
"ios" is the group in the hosts file that we want to run. <br/>
"-m raw" this is the raw module. <br/>
"-a" is used to pass the parameters which is command we want to execute. <br/>

The output returns alot of data. Filter the output to just display the version: <br/>
ansible -i hosts ios -m raw -a "show version" | grep "Version" <br/>
This will only display the strings that has "Version" in them.

We also need to display the device, else we won't know each version is for which device: <br/>
ansible -i hosts ios -m raw -a "show version" | grep "Version\|CHANGED" <br/>
"\|" means or. <br/>
This will display the line that has string "CHANGED", which also has the hostname in it. <br/>

Show Interface: <br/>
ansible -i hosts ios -m raw -a "show ip interface brief" <br/>
This will display so many interfaces. So filter it out to just display interfaces that are up: <br/>
ansible -i hosts ios -m raw -a "show ip interface brief" | grep "up\|CHANGED" <br/>

If you want to execute one command on multiple devices, the ansible ad-hoc command is helpful.

### How to Find Ansible Network Module
