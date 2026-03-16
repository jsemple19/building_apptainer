# building_apptainer

The following instructions are for building containers on a mac.

1 On your mac, install a virtual machine running ubuntu and install aptainer
2 Write the apptainer definition file using an ubuntu operating system as on the HPC
3 Build he container locally
4 Copy to server and use

## Apptainer on mac

### Installing ubuntu VM on UTM

1. [UTM](https://mac.getutm.app) is a free software for virtul machines on mac. Download and install it.
2. Download [ubuntu server](https://ubuntu.com/download/server/arm)

3. Create an ubuntu virtual machine on UTM:  Create a new Virtual Machine > Virtualize > Linux
4. Suggested specs:
  - 6-12Gb RAM
  - 4-6 cores
  - Boot from ISO image (the Ubuntu file you downloaded),
  - Storage space: 40-64Gb
  - Add a shared directory: this allows you to transfer files in and out of the VM
  - Enable OpenSSH server
  - Create a username and password

5. When finished - reboot. You might find that reboot just starts the installation all over again, in which case in UTM click on the VM, and then click on the button at the top right (edit VM) and disconnect the .iso file from the drives.
6. to turn off the VM, type `sudo poweroff`.
7. Note: UTM virtualization does not allow sharing clipboard sharing, which is annoying when cutting pasting commands, but you can ssh into the VM from a mac terminal, and this allows you to copy paste.


### ssh into VM

To ssh into vm from mac terminal, you need to type: `username@vm_ip`, e.g. `jenny@192.168.64.4`.  The ip is displayed as part of the welcome message when you log in to the VM.  If not, you might have to enable your ssh, or check ip manually. 

1. start your VM and login.
2. Check status with `systemctl status ssh` - it should say "active (running)".
3. Be sure ssh is installed: `sudo apt update
sudo apt install openssh-server`

4. enable ssh manually: `sudo systemctl start ssh
5. Make sure ssh is enabled at boot `sudo systemctl enable ssh`
6. use `ip a` to list ip.

### installing apptainer in vm
```
sudo apt update
sudo apt install -y build-essential libseccomp-dev pkg-config squashfs-tools cryptsetup runc uidmap

export VERSION=1.4.5
wget https://github.com/apptainer/apptainer/releases/download/v${VERSION}/apptainer_${VERSION}_amd64.deb
sudo apt install ./apptainer_${VERSION}_amd64.deb
```

