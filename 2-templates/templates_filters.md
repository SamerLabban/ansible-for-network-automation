### Templating
Templating means dynamic. <br/>
Ansible use Jinja2 templating  <br/>
Ref: <br/>
https://jinja.palletsprojects.com/en/3.0.x/  <br/>
https://jinja.palletsprojects.com/en/2.11.x/  <br/>

Why Templates? <br/>

Scenario 1: <br/>
Requirements:
- Use copy module to copy the cfg file to remote inventory host. Destination directory is /etc/app/default.cfg
- Execute command: app --f /etc/app/default.cfg
Imagine one day later we want to change the file name from default.cfg to something else. We'll need to fins all the places where this file exists to change. Thus a better practice for a such a scenario is to create variables and use a variable to name the file.

Scenario 2: <br/>
Requirement: Use copy module to copy the cfg file to remote inventory host. Make sure the ip is the remote inventory host ip. <br/>
You have the configuration file below:
```
[default]
version=1.0

[address]
tcp=5000
ip=192.168.1.1
```
So in above case, instead of prepare a configuration file for each destination ip address / host, we use a template, and populate it with the ip address to generate the config file. 

site.yml file without variables:
```
---
- hosts: web
  gather_facts: no
  tasks:
    - name: make sure the path exist
      become: yes
      file:
        path: /etc/app/
        state: directory
        owner: vagrant
    - name: copy cfg file
      template:
        src: test.cfg
        dest: /etc/app/test.cfg
```
Run the "site.yml" playbook. <br/>
Commands:
- ls
- more hosts
- Run this command to check if the "app" file exists at location "/etc": ansible -i hosts web -m raw -a "ls /etc"
- Run the ansible playbook on template file: ansible-playbook -i hosts site.yml
- Run this command to check if the folder got created: ansible -i hosts web -m raw -a "ls /etc"
- Run filter command to check the content of the test.cfg file: ansible -i hosts web -m raw -a "cat /etc/app/test.cfg"

Now use variable for the file and path names since these might change in the future. <br/>
Define the variables in the "hosts" file:
```
[web:vars]
cfg_path=/etc/app
cfg_file_name=test.cfg
cisco_cfg_file_name=cisco.cfg
```
And use these variables in the site.yml playbook:
```
---
- hosts: web
  gather_facts: no
  tasks:
    - name: make sure the path exist
      become: yes
      file:
        path: "{{ cfg_path }}"
        state: directory
        owner: vagrant
    - name: copy cfg file
      template:
        src: "{{ cfg_file_name }}"
        dest: "{{ cfg_path }}/{{ cfg_file_name }}"
    - name: print inventory hostname
      debug:
        var: inventory_hostname
```
Note to use a vraible use: "{{ <variable> }}" <br/>
Delete the config file from destination host:
```
ssh vagrant@192.168.205.10
sudo rm -rf /etc/app/
exit
```
Run the playbook and check if the folder and config files are created:
```
ansible-playbook -i hosts site.yml
# Verify the file is created
ssh vagrant@192.168.205.10
cd /etc/app/
ls
# check content of the file
more test.cfg
# Remove the app directory
cd ..
sudo rm -rf app/
exit
```

Do the test below:
- Change the file name from test.cfg to app.cfg
- Update the following variable in hosts file: cfg_file_name=app.cfg
- Execute the playbook: ansible-playbook -i hosts site.yml
- Go to the destination host and confirm the app.cfg file is created:
```
ssh vagrant@192.168.205.10
cd /etc/app/
ls
# check content of the file
more app.cfg
exit
# You can also check file content by doing:
ansible -i hosts web -m raw -a "more /etc/app/app.cfg"
```

### Jinja2 Filters
- Use Jinja2 filters to process and reformat the values of variables.
- Some filters are provided by the Jinja2 language itself, and others are extensions included with Ansible.
- Documentation: https://docs.ansible.com/ansible/latest/user_guide/playbooks_filters.html

How to use Jinja2 Filters:
- To apply a filter to a variable:
    - Reference the variable, but follow its name with a pipe character.
    - After the pipe character, add the nameof the filter you want to apply.
    - You can chain multiple filters in a pipeline.
- Example-1, the capitalize filter capitalizes the first letter of a string
    - Assume the value of myname variable is tom, and you have the following Jinja2 statement {{ myname | capitalize }}
    - The result should be Tom

### Jinja2 Template and Cisco cfg
The following is one example using Jinja2 template to configure BGP on Cisco router
```
router bgp {‌{local_asn}}
 neighbor {‌{bgp_neighbor}} remote-as {‌{remote_asn}}
!
 address-family ipv4
  neighbor {‌{bgp_neighbor}} activate
exit-address-family
```

There are some variables in this template
```local_asn, bgp_neighbor, remote_asn```
These variables can be setted from Ansible like host vars, groups var

