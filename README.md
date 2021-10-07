# ansible-for-network-automation

### Windows - WSL setup

Because Ansible Controller Should be Linux/Unix host, so
For Windows User, before you continue to the next video, please follow the instructions from this link https://docs.microsoft.com/en-us/windows/wsl/install-win10 to install windows WSL (Windows Subsystem for Linux)
We suggest you chose:
1. install Ubuntu 20.04 LTS
2. install windows terminal
And also there are some very nice videos on YouTube, please search "windows WSL" on YouTube and get some references.
For example https://www.youtube.com/watch?v=X-DHaQLrBi8

On Linux WSL, update Linux source: sudo apt-get update
Install dependencies: sudo apt-get install python3-dev python3-vnev python3-pip pyhon3-wheel

### Create virtual environment

Note: Use ansible 2.9.1 or 2.9.5
To check versions of ansible go to github.com/ansible/ansible  --> click on tags and you should see all versions there.
We'll use version 2.9.1

Create virtual environment:  python3 -m venv ansible-2.9.1
Above command will create a folder "ansible-2.9.1" which is my virtual environment created.
Activate the virtual environment:  source ansible-2.9.1/bin/activate
Install required dependencies in virtual env:
pip install wheel
pip install ansible==2.9.1
(Note: if you do "pip install ansible==2.10.1", you notice that there is no stable release of version 2.10. You can see latest version in returned out us 2.10.0rc1, which means release candidate 1, so it is not stable release yet).
Verify ansible got installed: ansible --version
Start virtual environment:  source ansible-2.9.1/bin/activate
To stop virtual environment: deactivate

Note: You can test your code on mutliple versions of ansible by creating different virtual environments and installing a different ansible version on each.

### Important notes for Windows WSL User
Please install sshpass
(ansible-2.9.5) penxiao@WPU8L0042502:~$ sudo apt-get install sshpass

### Visual Studio as IDE
Install visual studio code
After installing visual studio, install extensions:
- ansible
- YAML
- remote development
The Remote Development extension pack allows you to open any folder in a container, on a remote machine, or in the Windows Subsystem for Linux (WSL) and take advantage of VS Code's full feature set.

### Visual Studio COde on Windows (WSL)
On Visual Studio, install extenion: Remote - WSL
The Remote - WSL extension lets you use VS Code on Windows to build Linux applications that run on the Windows Subsystem for Linux (WSL). You get all the productivity of Windows while developing with Linux-based tools, runtimes, and utilities.
Remote - WSL lets you use VS Code in WSL just as you would from Windows.

Now if you open WSL on Windows.
Create a folder: mkdir python-test
cd python-test/
code . --> this command will install visual code server inside WSL and will launch visual studio code at the folder your are in.
 
### Doc: VSCode Links and Extensions
VScode Link: https://code.visualstudio.com/
16 Unique VSCode Extensions Every Developer Should Have in 2020
https://blog.bitsrc.io/16-unique-vscode-extensions-every-developer-should-have-in-2020-c4dcdb74506a

### Prepare Network Devices/Topology
Options:
- Physical Devices
- Virtual Devices
  - GNS3
  - CSR1000v(locally or in the cloud)
  - Others
 
 - Make  sure you can connect your devices from your laptop through SSH!

### Prepare one Linux Machine
Options:
- Cloud Server (AWS, Azure)
- Linux Virtual Machine Locally
  - Vmware
  - Virtualbox
  - others

- Make sure you can connect Linux host from your laptop through SSH!

### Why Ansible 2.9.x?
If you want to use open source software, do not use the latest version, because although it'll have the latest features, but the risk is that it might have some bugs. SO for version v2.9 we have versions from v2.9.0 till v2.9.13, and anisble has been fixing bugs between all release v2.9.0 and v2.9.13. So v2.9.13 is stable and has fewer bugs than v2.10.1 forexample.
Another reason is that between version v2.9 and v2.10, there is a big difference, but playbooks from v2.9 can execute in v2.10

### Ansible Documentation
https://docs.ansible.com
In ansible docs, you can also choose the documentation based on the ansible version you are using.

### .ansible.cfg
Please create one .ansible.cfg file in your home directory  (~/)
Both in WSL and Mac terminal, just use cd ~ go to your home directory
$ cd ~
$ ls -la | grep ansible
-rw-r--r--   1 pengxiao  staff      62 Sep 23 23:29 .ansible.cfg
and create a file called .ansible.cfg with content as below
[defaults]
host_key_checking=False
deprecation_warnings=False
