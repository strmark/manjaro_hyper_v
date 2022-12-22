# Running Manjaro Linux on Hyper-V.

Installation of Manjaro XFCE Linux on Hyper-V.

Create a new VM and disable the "Enable Secure Boot"

Start the live-cd version of Manjaro it will stop at "Finished TLP system startup/shutdown."

Press "Ctrl-Alt-F2" to start a new terminal window. Login with manjaro/manjaro. Install the video drivers:
``` bash
sudo pacman -Sy xf86-video-fbdev
startxfce4
```
You will now launch to the desktop and you are able to launch the installer. Follow the installer and at the end shutdown the vm to remove the iso from the dvd-drive.

The VM will not boot to a desktop since the installed machine does not have the videodrivers. Again after startup open a terminal window with "Ctrl-Alt-F2"

Login with your new user and password. (the username and password you provided during the installation on the VM) Install the video drivers:
``` bash
sudo pacman -Sy xf86-video-fbdev
startxfce4
```

Open a terminal:
update the database and environment from pacman
``` bash
sudo pacman -Syyu 
```
Install the video drives again.
``` bash
pacman -Sy base-devel git wget curl vim yay
```
Run the scripts makepkg.sh and install-config.sh
``` bash
./makepkg.sh
```

Run the install-config to configure xrdp.
``` bash
sudo ./install-config.sh
```

Edit the local .xinitrc and change it to:
```
#!/bin/bash
exec startxfce4
or
#!/bin/bash
dbus-launch startxfce4
```

Edit the grub configuration:
``` bash
sudo vi /etc/default/grub
or 
sudo nano /etc/default/grub
````
Add `systemd.unit=multi-user.target` to the end of line GRUBCMD_LINE_LINUX_DEFAULT this will boot the system to the command prompt. So only the xrdp will run a window manager.

Save the file and update grub
``` bash
sudo update-grub
```
Run in a Windows Powershell on your Windows system as administrator:
```
Set-VM -VMName "MachineName" -EnhancedSessionTransportType HvSocket
```

Reboot the system.
