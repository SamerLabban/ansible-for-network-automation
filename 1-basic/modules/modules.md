### Modules
A module is a reusable, standalone script that Ansible runs on your behalf, either locally or remotely. Modules interact with your local machine, an API, or a remote system to perform specific tasks like changing a database password or spinning up a cloud instance. Each module can be used by the Ansible API, or by the ansible or ansible-playbook programs. A module provides a defined interface, accepts arguments, and returns information to Ansible by printing a JSON string to stdout before exiting. <br/>
For example: <br/>
- Mysql User module: manage mysql users
- Cisco IOS Command module: execute cisco ios commands.

Test your hosts file: <br/>
- Activate virtual environment: source vir/ansible-2.9/bin/activate
- cd 1-basic/modules
- ls
- Check hosts file content: more hosts
- Make sure you can access the machine defined in the hosts file first by doing ssh: ssh vagrant@192.168.205.10 <br/>
ansible_password=vagrant
- Execute ansible command: ansible -i hosts all -m ping <br/>
ping: used to ping the hosts defined in ansible hosts file. If all is good, you'll get success command.
- Execute specific command: <br/>
ansible - hosts all -m raw -a "date" <br/>
"raw" is a module, we'll use that module to execute some command on our remote inventory host. We use "-a" and specify the command to execute. <br/>
"date" will show the current date on the host. <br/>
To list all files and folders in current directory on host: <br/>
ansible -i hosts all -m raw -a "ls -a" <br/>
If you do the following, you'll get the same result: <br/>
ssh vagrant@192.168.205.10 <br/>
ls -a <br/>

### Ansible ad-hoc command
- An Ansible ad-hoc command uses the /usr/bin/ansible command-line tool to automate a single task on one or more managed nodes.
- ansible [pattern] -m [module] -a "[module options]"
   - ansible -i hosts all -m ping
   - ansible -i host web -m raw -a "pwd"
Note: Some modules like "raw", you'll need to pass arguments or options such as "pwd". <br/>
Other modules like "ping" do not take any arguments.

How do I now what module to use? <br/>
Open Ansible documentation and search for "Module Index": https://docs.ansible.com/ansible/2.9/modules/modules_by_category.html <br/>
You'll see all modules there are grouped. For example you'll see module "raw" in group "Commands modules". In "System Modules" group, you'll see the "ping" module. <br/>
Group "Files modules" has a module called "copy" which is used to copy files o remote locations. <br/>
https://docs.ansible.com/ansible/2.9/modules/copy_module.html#examples <br/>
You'll see that the copy module takes several arguments: src / dest / owner / group / node

Try copying a file to a destination host: <br/>
- Activate virtual environment: source vir/ansible-2.9/bin/activate
- cd 1-basic/modules
- ls
- more hosts
- ansible -i hosts all -m copy -a "src=./test.txt dest=/home/vagrant/test.txt"

Now if we go into the destination host, we'll see the file copied there:
- ssh vagrant@192.168.205.10
- cd /home/vagrant
- ls
- ls -lh --> to see file creation details
- cat test.txt --> to see file content

If you modify the source file and execute the anisble command again, the file at the destination will be over-written.

Note: Add your Cisco router to the hosts file, and give it a group name. Add the router DNS name or ip address, along with the SSH username and password. 
```
[csr1000v]
csr1.gitlab-demo.com ansible_user=cisco ansible_password=....
```
After that use, the ansible command to execute "show run" command on the router.
```
ansible -i hosts csr100v -m raw -a "show run"
```
