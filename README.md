# linux4dedede
**Almost completely from [ading2210/shimboot](https://github.com/ading2210/shimboot).** Streamlined process to get Linux up and running as easily as possible on enrolled dedede boards.


## Quickstart(Default installation: Debian XFCE):
**On target device, go to `chrome://version` and read platform section. Provided image will only work if the last word in the `Platform:` section is `dedede`**
**Note that booting Linux from ChromeOS and vice versa will powerwash target device. Most features should work, EXCEPT speakers. :(**

### The following should be done on a **seperate** device:
1) Download `shimboot_dedede.zip` from original repo [here](https://github.com/ading2210/shimboot/releases/download/v1.2.1/shimboot_dedede.zip).
2) Flash shim image to USB stick with more than 8GB storage. Ideally with [this](https://chromewebstore.google.com/detail/chromebook-recovery-utili/pocpnlppkickgojjlmhdmidojbmbodfm) or `dd` on Linux.
3) Verify image and remove USB from computer.

### The following should be done on the **target** device:
1) Power on, and press **esc+refresh+power**. Device should reboot in recovery mode.
2) Press **ctrl+d** and hit **enter**. Device should be in temporary developer mode.
3) Press **esc+refresh+power** again. Device will go into recovery mode with developer mode temporarily enabled.
4) Plug in flashed USB drive, and wait for Debian to boot.
5) Login with `user`/`user`. You can change this later.
6) Run `sudo expand_rootfs`, with given password on above line. This will expand rootfs partition to fill up entire USB disk.
7) Running `passwd user` will change user password. The root user is disabled by default.

## Customizing image with different  supported distros/desktops:
### Following should be done prior to flashing USB on non-target device. 
1) Clone `dedede` from [here](https://chrome100.dev)
2) Cd into cloned repo
3) Run any of the following commands to build image according to needs:

   **To change distro:**  \
   `sudo ./build_complete.sh dedede release=unstable` (Debian Rolling Release)   \
   `sudo ./build_complete.sh dedede distro=alpine` (Experimental Alpine Linux)

   **To change desktop environment:**
   `sudo ./build_complete.sh grunt desktop=lxde`  \(`lxde` can be replaced with any one of the following: `gnome`,`xfce`,`kde`,`gnome-flashback`,`cinnamon`,`mate`, or `lxqt`)

5) Continue from step 2) on first quickstart section. The image you just built should be the one you end up flashing.

## Misc:
### Booting into ChromeOS again:
Reminder that this is a liveboot that doesn't touch internal storage by default. 
Power off from Linux and unplug USB. Then, power on and press **esc+refresh+power** to powerwash and exit recovery into normal mode.
To go back to Linux, follow steps on second quickstart section.

### Bluetooth:
To get bluetooth up and running install the following from terminal:   \
`sudo apt-get install bluetooth blueman`   \
You can use bluetooth audio output for sound as speakers aren't currently supported.

### Smartphone Audio:
Another way to get audio despite unsupported speakers is by using your smartphone as a speaker via Wi-Fi.   \
Download [audiorelay](https://audiorelay.net)  .deb file on target device and the app on your phone.    \   
To install the .deb file, right click on your downloads folder and select open Terminal here. Then, type in:   \
`sudo dpkg -i <package_name>`    \
*You may need the libflac8 dependency from [here.](http://ftp.de.debian.org/debian/pool/main/f/flac/libflac8_1.3.3-2+deb11u2_amd64.deb)   \
Now, you can just open audiorelay on target device and connect to it on your smartphone via the app.

### Rootfs compression:
Run the following in terminal:  \
`sudo ./build_rootfs.sh data/rootfs bookworm`  \
`sudo ./build_squashfs.sh data/rootfs_compressed data/rootfs path_to_shim`  \
`sudo ./build.sh image.bin path_to_shim data/rootfs_compressed`

### Wifi Troubleshooting
Run the following in terminal:  \
`nmcli connection edit <your connection name>`  \
`set 802-11-wireless-security.pmf disable`  \
`save`  \
`activate`  

## General Disclaimer
You are using these methods and/or softwares at your own risk. Doing stupid things can and will brick your device. Only you are responsible for any damage, irreversible changes, or improvements made to your device. :)
