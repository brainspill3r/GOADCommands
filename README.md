# GOADCommands
Notes For Installing and avoiding errors. 


Helpful Links & Credits to. 
https://github.com/Orange-Cyberdefense/GOAD/blob/main/docs/install_with_vmware.md

https://github.com/quincyntuli/GOAD-v2-Installation-Notes

https://www.youtube.com/watch?v=5ggZHnp7eXc


Requirements 
```
300GB 
24 GB Ram (24576)
8 Processors - 4 x 2 
VTx - Ticked Otherwise your run into countless errors. 

```


What is ansible
```
Ansible is an open-source automation tool that simplifies configuration management, application deployment, and task automation. It allows users to define and manage **infrastructure as code,** enabling them to automate repetitive tasks, streamline complex workflows, and ensure consistency across multiple servers or devices.
```

What is Vagrant 
```
Vagrant is an open-source tool for building and managing virtualized development environments. It provides a command-line interface to configure and deploy reproducible virtualized environments on various platforms, such as VirtualBox, VMware, and others.
```


### **Enable Nested Virtualization in the VMware Host (Outer VM):**

- Ensure that your VMware host (the outer VM) allows nested virtualization. This setting is often referred to as "Expose hardware-assisted virtualization to the guest OS" or something similar in VMware settings. It might be found in the virtual machine settings under the "Processor" or "CPU" section

  VT-x (Virtualization Technology for x86) is an Intel processor technology that provides hardware-assisted virtualization. It's a crucial feature in nested virtualization scenarios where you run a virtual machine (VM) inside another VM. (Click the small tick box in VMWare) 
```


### Step 2: Add VirtualBox Directory to PATH
To add the `VBoxManage` binary to the `PATH` environmental variable, you need to find the directory where VirtualBox is installed and then add that directory to your `PATH`. Here are the general steps:
Once you've identified the directory containing `VBoxManage`, you can add it to the `PATH`:

#### On Linux:

```bash
which VBoxManage 

export PATH=$PATH:/usr/bin/VBoxManage
```

```shell

300GB 
24 GB Ram  
VTx - Ticked Otherwise your run into countless errors. 


sudo apt update
sudo apt upgrade
```

### Install Virtualbox

```shell
sudo apt install virtualbox
sudo apt-get install git-all
sudo apt install htop
```

### [Install Vagrant

```shell
wget https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
sudo apt install ./vagrant_2.2.19_x86_64.deb
vagrant --version


wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg |
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list |
sudo apt update && sudo apt install vagrant

```

### Install Python pip

```shell
sudo apt install python3-pip
pip3 --version
```

### Install Python Virtual Environment
```shell
sudo apt install python3-venv
```

### Clone GOAD v2

```shell
git clone https://github.com/Orange-Cyberdefense/GOAD.git
```

if git is not installed, then install it first

```shell
sudo apt-get install git-all
```

### Create the Virtual Environment

```shell
python3 -m venv venvGOAD
```

### Activate the Virtual Environment

```shell
cd GOAD/ansible
source ~/venvGOAD/bin/activate
```

### Install ansible module

```shell
./goad.sh -t check -l GOAD -p virtualbox -m local
python3 -m pip install --upgrade pip
python3 -m pip install ansible-core==2.12.6
python3 -m pip install pywinrm
export PATH=$PATH:/usr/bin/VBoxManage

```

### Install galaxy requirements

```shell
ansible-galaxy install -r requirements.yml (from within virtual enviroment)

```

### Using Vagrant 

First, we let vagrant setup the 5 instances. The ISO will be downloaded and the VMs will be setup. If a local copy of the .iso already exists then this download part will be skipped and the machine will be imported from the .iso and built.

```shell
cd ad/GOAD/providers/virtualbox
vagrant up
```

At this stage the instructions tell us to play the plabook for all playbooks by executing.  However you can just keep running the main.yml file and all errors will eventually be fixed. 

```shell
ansible-playbook main.yml
```

As copied from the official github, those steps are ...

```
cd ansible/
ansible-playbook -i ../ad/GOAD/data/inventory -i ../ad/GOAD/providers/virtualbox/inventory main.yml


Or individually following the list below.

ansible-playbook -i ../ad/GOAD/data/inventory -i ../ad/GOAD/providers/virtualbox/inventory servers.yml


```shell
ansible-playbook build.yml        # Install stuff and prepare vm
ansible-playbook ad-servers.yml   # create main domains, child domain and enroll servers
ansible-playbook ad-trusts.yml    # create the trust relationships
ansible-playbook ad-data.yml      # import the ad datas : users/groups...
ansible-playbook servers.yml      # Install IIS and MSSQL
ansible-playbook ad-relations.yml # set the rights and the group domains relations
ansible-playbook adcs.yml         # Install ADCS on essos
ansible-playbook ad-acl.yml       # set the ACE/ACL
ansible-playbook security.yml     # Configure some securities (adjust av enable/disable)
ansible-playbook vulnarabilities.yml # Configure some vulnerabilities
```

```shell

When you finish playing you could do :

vagrant halt # will stop all the vm

To just relaunch the lab (no need to replay ansible as you already do that in the first place)

vagrant up   # will start the lab

```
![image](https://github.com/brainspill3r/GOADCommands/assets/68113403/07d1616a-8f0b-4de3-ada6-d4de4f3c6f61)

![image](https://github.com/brainspill3r/GOADCommands/assets/68113403/65dd8e58-ba77-48d6-9438-b35d351f79be)

![image](https://github.com/brainspill3r/GOADCommands/assets/68113403/db361fff-86f2-4e15-a072-4ed93bc94569)


If you see this error - likley you haven't selected VT-x 

![image](https://github.com/brainspill3r/GOADCommands/assets/68113403/8ccc263d-ebef-4b55-92cb-f9fa6b2b69ac)





