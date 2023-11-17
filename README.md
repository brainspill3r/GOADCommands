# GOADCommands
Notes For Installing and avoiding errors 


Helpful Links
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


### 1. **Enable Nested Virtualization in the VMware Host (Outer VM):**

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

![[Pasted image 20231114172905.png]]

![[Pasted image 20231114192337.png]]

```shell

300GB 
24 GB Ram  
VTx - Ticked Otherwise your run into countless errors. 


sudo apt update
sudo apt upgrade
```

### [03 Install Virtualbox](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#03-install-virtualbox)

```shell
sudo apt install virtualbox
sudo apt-get install git-all
sudo apt install htop
```

### [04 Install Vagrant](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#04-install-vagrant)

```shell
wget https://releases.hashicorp.com/vagrant/2.2.19/vagrant_2.2.19_x86_64.deb
sudo apt install ./vagrant_2.2.19_x86_64.deb
vagrant --version


wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor | sudo tee /usr/share/keyrings/hashicorp-archive-keyring.gpg |
echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list |
sudo apt update && sudo apt install vagrant

```

### [05 Install Python pip](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#05-install-python-pip)

```shell
sudo apt install python3-pip
pip3 --version
```

### [06 Install Python Virtual Environment](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#06-install-python-virtual-environment)

```shell
sudo apt install python3-venv
```

### [07 Clone GOAD v2](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#07-clone--goad-v2)

```shell
git clone https://github.com/Orange-Cyberdefense/GOAD.git
```

if git is not installed, then install it first

```shell
sudo apt-get install git-all
```

### [08 Create the Virtual Environment](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#08-create-the-virtual-environment)

```shell
python3 -m venv venvGOAD
```

### [09 Activate the Virtual Environment](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#09-activate-the-virtual-environment)

```shell
cd GOAD/ansible
source ~/venvGOAD/bin/activate
```

### [10 Install ansible module](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#10-install-ansible-module)

```shell
./goad.sh -t check -l GOAD -p virtualbox -m local
python3 -m pip install --upgrade pip
python3 -m pip install ansible-core==2.12.6
python3 -m pip install pywinrm
export PATH=$PATH:/usr/bin/VBoxManage

```

### [12 Install galaxy requirements](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#12-install-galaxy-requirements)

```shell
ansible-galaxy install -r requirements.yml (from within virtual enviroment)

```

### [13 Lets play](https://github.com/quincyntuli/GOAD-v2-Installation-Notes#13-lets-play)

First, we let vagrant setup the 5 instances. The ISO will be downloaded and the VMs will be setup. If a local copy of the .iso already exists then this download part will be skipped and the machine will be imported from the .iso and built.

```shell
cd ad/GOAD/providers/virtualbox
vagrant up
```

At this stage the instructions tell us to play the plabook for all playbooks by executing

```shell
ansible-playbook main.yml
```

My recommendation is to play the individuals plays books one by one to give each VM a chance to complete the build process so as to respond properly to the next set of tasks for each playbook.

As copied from the official github, those steps are ...

```
cd ansible/
ansible-playbook -i ../ad/GOAD/data/inventory -i ../ad/GOAD/providers/virtualbox/inventory main.yml


Or individually folloing the list below. However you can just keep running the main.yml file and all errors will eventually be fixed. 

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
![[Pasted image 20231114193900.png]]

![[Pasted image 20231114194738.png]]

![[Pasted image 20231114195236.png]]
![[Pasted image 20231114212839.png]]
![[Pasted image 20231114213153.png]]

![[Pasted image 20231114203607.png]]
```shell

When you finish playing you could do :

vagrant halt # will stop all the vm

To just relaunch the lab (no need to replay ansible as you already do that in the first place)

vagrant up   # will start the lab

```



