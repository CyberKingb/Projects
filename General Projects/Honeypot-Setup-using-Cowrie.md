## Honeypot Setup using Cowrie ##
**Step 1: Set Up Your Virtual Environment**

To prevent any actual compromise of your system, it’s best to run the honeypot inside a virtual machine (VM).


- Install VirtualBox or VMware on your system.
- Download [VirtualBox](https://www.virtualbox.org/) 
- [Download VMware Workstation Player](https://www.vmware.com/products/desktop-hypervisor/workstation-and-fusion) 
- **Install Ubuntu** (or any other Linux distro) on your VM. 
- You can download the ISO file for Ubuntu [here](https://ubuntu.com/download/desktop)
- Allocate sufficient resources for the VM (e.g., 2GB RAM, 20GB storage).
  
I already have all Virtualbox and Kali Linux  installed on my computer so i wont be going through that process here.

**Step 2: Install Required Dependencies**

Once the VM is set up and running, install the required dependencies for Cowrie.

**1.** Update your system and install the necessary packages:
```
sudo apt-get update
```

- <img width="666" height="482" alt="image" src="https://github.com/user-attachments/assets/c7ea95ed-6f05-43f4-8bd5-a8413ef8fed3" />

```
sudo apt-get upgrade
```

- <img width="659" height="281" alt="image" src="https://github.com/user-attachments/assets/034e8d4a-5581-43b5-b147-2ff43c34ebce" />

```
sudo apt-get install python3 python3-venv git
```
- 
