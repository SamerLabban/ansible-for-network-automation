### Ansible Playbook
- Playbooks are Ansible's configuration, deployment, and orchestration language.
- Each playbook is composed of one or more 'plays' in a list. One 'play' is a collection of one or more 'tasks', a 'task' is a single action that you want to execute through Ansible.

### Ansible Playbook demo
- Prepare one Centos7 host
- Install Nginx
- Copy index.html from local to remote /usr/share/nginx/html/index.html
- Start/restart nginx service

Before you run the play book, check if nginx service is installed on your remote device:
- Satrt virtual environment "ansible-2.9" created before.
- ssh vagrant@192.168.205.10
- sudo systemctl status nginx
- You'll see output: Unit nginx.service could not be found. --> meaning nginx is not installed on the Linux host.

Execute the anisble playbook:
```
ansible-playbook -i hosts site.yml
```
After running the playbook, if you open the ip address of the remote machine in the browser, you'll see the "Hello Ansible" html page.

If we execute the ansible playbook again, you'll see we have zero changes, since changes are already done.

If we change the html page and re-run the playbook, the copy command that copies the html file will be executed again.


### Ansible Facts
When ansible playbook executes, the "Gathering Facts" task gets executed firt by default.

Ansible facts are data related to your remote systems, including operating systems, IP addresses, attached filesystems, and more. You can access this data in the ansible_facts variable. By default you can also access some Ansible facts as top-level variables with the ansible_ prefix.

By default, Ansible gathers facts at the beginning of each play. If you do not need to gather facts (for example, if you know everything about your systems centrally), you can trun off fact gathering at the play level to improve scalability.
```
- hosts: whatever
  gather_facts: no
```

#### Set custom fact
The setup module in Ansible automatically discovers a standard set of facts about each host. If you want to add custom values to your facts, you can write a custom facts module, set temporary facts with a set_fact task, or provide permanent custom facts using the facts.d directory.
```
- name: set facts
  set_fact:
    my_fact:
    - 10
    - 20
- name: debug
  debug:
    var: my_fact
```
ABove I created a custon fact called "my_fact" and passed a list [10,20] to it.

### ansible-doc command
If we want to use an ansible module, but do not know what it does. We can either:
- Check ansible online documentation
- Or use ansible-doc command to check the documentation offline. After installing ansible on our machine, it installed with it the documentation. <br/>
ansible-doc -t module <module_name>

Example:
```
ansible-doc -t module copy
ansible-doc -t module copy
```
