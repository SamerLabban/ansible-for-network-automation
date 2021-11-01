### Loops and Conditionals
Examples:
- How about if we want to create 10 users (test1, test2, ...). We use loops.
- How to check host Linux distribution? We use conditional.

Create loops.yml playbook: 
```
---
- name: loop test
  hosts: web
  become: yes
  tasks:
    - name: "create local user"
      user:
        name: test1
```
Run the playbook:  ansible-playbook -i hosts loops.yml  <br/>
SSH to the remote hosts confirm that the users are created:
- ssh vagrant@192.168.205.10
- more /etc/passwd --> you'll see there one user called test1

Add more users in loops.yml and executte the playbook:
```
---
- name: loop test
  hosts: web
  become: yes
  tasks:
    - name: "create local user"
      user:
        name: test1
    - name: "create local user"
      user:
        name: test2
    - name: "create local user"
      user:
        name: test3
```
Or better approach is to use loops as below:
```
---
- name: loop test
  hosts: web
  become: yes
  tasks:
    - name: "create local user"
      user:
        name: "{{ username }}"
      loop:
        - test1
        - test2
        - test3
        - test4
      loop_control:
        loop_var: username
```
Note: <br/>
We use the following to change the default variable name from "item" to another name (example "username"):
```
      loop_control:
        loop_var: username
```


To learn more about loops, refer to the ansible documentation: https://docs.ansible.com/ansible/latest/user_guide/playbooks_loops.html


### Conditions
Use case: <br/>
CentOS uses yum to install software. <br/>
Ubuntu uses apt-get. <br/>
So depending on the host we are running the command on, we'll use the corresponding command.

Run the playbook: <br/>
ansible-playbook -i hosts condition.yml <br/>
After running the playbook, you'll see that it skips host 192.168.205.11, because it has ubuntu and not Centos.

To learn more about conditionals, refer to the ansible documentation: https://docs.ansible.com/ansible/latest/user_guide/playbooks_conditionals.html
