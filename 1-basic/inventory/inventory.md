### Inventory
Inventory is the target host in our infrastructure which we want to operate by ansible. We put all the host information into one file called inventory file. <br/>
The default inventory file is /etc/ansible/hosts <br/>
We will not use this default inventory file, but we will create our own inventory file.

The inventory file is a text file that has an INI-like format file. <br/>
You can add your hosts into groups, example:
```
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.exampe.com
```

You can call a specific group from the above inventory file, or use default group "all" which will run on all hosts/groups in the file.

There are two default groups: "all" and "ungrouped". <br/>
"all" contains every host. <br/>
"ungrouped" contains all hosts that don't have another group aside from "all". --> in above example, it'll be "mail.example.com"

### Variables
We can variables to hosts and groups. <br/>
You can variables for specific groups, example:
```
[csr_1000v]
csr1.test.com version=17.1 location=101
csr2.test.com version=17.2
csr3.test.com

[csr_1000v:vars]
a=1
b=2
c=3
version=17.0
```
Note: the host level variable has higher priority over group variable, as for variable "version" above.

### Pre-defined Host Variables
You can find these variables from ansible documentation. All these variables start with "ansible_". <br/>
Link: https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html

Note: avoid using name "ansible_" naming convention in your custom variables.

