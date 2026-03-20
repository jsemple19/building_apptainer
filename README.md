# building_apptainer

The following instructions are for building containers on a mac.

1. On your mac, install a virtual machine running ubuntu and install aptainer
2. Write the apptainer definition file using an ubuntu operating system as on the HPC
3. Build he container locally
4. Copy to server and use

## Apptainer on mac

### Installing ubuntu VM on UTM

1. [UTM](https://mac.getutm.app) is a free software for virtul machines on mac. Download and install it.
2. Download [ubuntu server](https://ubuntu.com/download/server). Make sure to get the version suitable for AMD, not ARM architecture.
4. Create an ubuntu virtual machine on UTM:  Create a new Virtual Machine > Emulate > Linux, then make sure to select x64 architecture.
5. Suggested specs:
  - 6-12Gb RAM
  - 4-6 cores
  - Boot from ISO image (the Ubuntu file you downloaded),
  - Storage space: 40-64Gb
  - Add a shared directory: this allows you to transfer files in and out of the VM
  - Enable OpenSSH server
  - Create a username and password

5. When finished - reboot. You might find that reboot just starts the installation all over again, in which case in UTM click on the VM, and then click on the button at the top right (edit VM) and disconnect the .iso file from the drives.
6. to turn off the VM, type `sudo poweroff`.
7. you want to allow clipboard sharing (only in emulate and not virtualize mode) so you can cut and paste commands from outside the VM. Setting it up didn't work for me but you can ssh into the VM from a mac terminal, and this allows you to copy paste.

### sharing clipboard with VM
Make sure that sharing clipboard is enabled by closing your VM, and choosing edit at the top right courner of UMT. Choose Sharing from the menu.

Install SPICE guest tools inside the VM:

```
sudo apt update
sudo apt install spice-vdagent
```
reboot the VM:
```
reboot
```
this didn't work for me. 


### ssh into VM

To ssh into vm from mac terminal, you need to type: `username@vm_ip`, e.g. `jenny@192.168.64.4`.  The ip is displayed as part of the welcome message when you log in to the VM.  If not, you might have to enable your ssh, or check ip manually. 

1. start your VM and login.
2. Check status with `systemctl status ssh` - it should say "active (running)".
3. Be sure ssh is installed:
   ```
   sudo apt update
   sudo apt install openssh-server
   ```

5. Enable ssh manually: `sudo systemctl start ssh`
6. Make sure ssh is enabled at boot `sudo systemctl enable ssh`
7. use `ip a` to list ip.

### installing apptainer in vm
```
sudo apt update
sudo apt install -y build-essential libseccomp-dev pkg-config squashfs-tools cryptsetup runc uidmap
sudo apt install apptainer

# alternative manual install (not necessary)
#export VERSION=1.4.5
#wget https://github.com/apptainer/apptainer/releases/download/v${VERSION}/apptainer_${VERSION}_amd64.deb
#sudo apt install ./apptainer_${VERSION}_amd64.deb
```
## creating def file for your software

In the header you must specify the same ubuntu as on the server:

```
Bootstrap: docker
From: ubuntu:24.04
```
Try asking microsoft copilot to create def for you based on installation instructions.

### convert build container from .def file

```
sudo apptainer build deconwolf.sif deconwolf.def
```

### test it
```
sudo apptainer exec deconwolf.sif dw --help
```

### copy container to mac and then to server
From the mac terminal (not ssh'd into the UTM) go to the folder where you want the .sif file and type:
```
scp jenny@192.168.64.4:/home/jenny/deconwolf.sif ./
```

### run container with gpu support
```
apptainer exec --nv deconwolf.sif dw --help
```




