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
Verify ansible got installed: ansible --version
Start virtual environment:  source ansible-2.9.1/bin/activate
To stop virtual environment: deactivate

