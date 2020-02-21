# Pico8-Script-for-GPi-Case
(Unofficial) Script to make the GPi Case run Pico8.

Like Pico8? Wanna install it on top of a fresh "Raspbian"-Image? PICOPI isn't working anymore? Than that's for you! This Script automates the Installation of Pico8 on a GPi Case; after that, the GPi Case will boot directly into PICO8-Splore.

Of course, you need PICO8 to do that; PICO8 isn't free; you can buy PICO8 here: https://www.lexaloffle.com/pico-8.php


1. Download Raspbian _Lite_ from:
⋅⋅⋅https://www.raspberrypi.org/downloads/raspbian/
⋅⋅⋅Write the Image to the SD-Card; you can use Win32DiskImager or Etcher for this. (Win32DiskImager needs the "img"-file inside the Archive, Etcher can handle the "img.gz"-Archive directly)

2. Download wiringpi from: https://archive.raspberrypi.org/debian/pool/main/w/wiringpi/
(I think you can use the newest Version; while typing this, 2.50 was the newest Release). Copy that "wiringpi_{version}_armhf.deb"-File to the "boot"-Partition.

3. On Pi Zero W (including Wifi): Optional but recommended: Create an empty file called "ssh" on the "boot"-Partition to allow SSH-Connections (Windows-Users should watch because of the Filename-Extension). If you do so, don't forget to change the Password for User "pi" later!

4. On Pi Zero W (including Wifi): Optional but highly recommended: Create a file called "wpa_supplicant.conf" on the "boot"-Partition including these Lines:
```
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=<Your Country-Code>

network={
  ssid="<Your 2.4 GHz-WLAN-SSID>"
  psk="<Your Password>"
  key_mgmt=WPA-PSK
}
```
(Replace everthing from < to >)

5. Download the GPi Case-Patch from: http://download.retroflag.com/ (The left "Download"). These Zip-Archive contains a folder called "patch_files"; copy everything from that Folder to the "boot"-Partition and overwrite config.txt and the Folder "overlays".

6. Download PICO8 for Raspberry Pi and extract the ZIP-Archive to your "boot"-Partition ("D:\pico-8"...).

7. Optional but recommended: Create a Folder named "carts" on the Pico-8-Directory ("D:\pico-8\carts") and put some of your favourite Cartridges in there.

8. Edit "cmdline.txt", there should be a passage like
```
init=/usr/lib/raspi-config/init_resize.sh
```
Replace it with:
```
init=/bin/bash -c "mount -t proc proc /proc; mount -t sysfs sys /sys; mount /boot; source /boot/makepico8"
```

9. Download the file "makepico8" and copy that file to your "boot"-Partition:
https://raw.githubusercontent.com/Astorek86/Pico8-Script-for-GPi-Case/master/makepico8

#### Warning on Windows- and Mac-Users: This is a Shell-Script which contains LF as Newline. Do _NOT_ modify this File using Notepad! Please use a better Program that can save Files with LF as Newline, such as Notepad++ or Geany.

10. Remove the SD-Card safely from your PC, and boot the GPi Case with the same SD-Card. On first Boot, it should print some Text on Screen and will soon reboot. After Reboot, it should Boot directly into PICO8.


## So, what does this Script 'exactly'?
At first, you can directly look into the "makepico8"-File; it's just a simple Shell-Script.
* It creates a new User called "pico8" with a "Home" on `/boot/pico-8`,
* it changes the Permission on the "/boot"-Partition in /etc/fstab, because in Linux, only Root would be allowed to write on VFAT-Partitions. This Script changes that and gives Permission directly to User "pico8" and the Group "pico8".
* It modifies some systemd-files to allow Autologin for User "pico8".
* It modifies `/etc/sudoers` for the Ability to Shutdown for User "pico8".
* It does some more Things here and there to avoid printing Text on Screen while Booting. Because Aesthetics^^.


## Performance is too slow
You can open "config.txt" and Overclock as you wish. I added some Options on the Bottom of the File; these Options are disabled (remove the `#` to enable these). Again: Use it with Caution! (Personally I recommend this, but these Settings will also switch the Warranty-Bit on the Raspberry, and I won't do that explicitly. YOU need to do that^^.)


## Volume is too loud or too quiet
Open `pico-8/.profile`, there's a Section that will look like
```
amixer sset 'PCM' 80%
```
Replace the 80% to your needs (IMHO 100% is too loud, but that's just my Opinion...).


## How do I change ssh-Password?
Login to SSH (if you don't know the IP, try its Hostname: `raspberrypi`). After Login (Username `pi`, Password `raspberry`), type `passwd`.


## Which Password has User "Pico8"?
No Password. This is intended; it's for Autologin and running PICO8 only. It's (to my Knowledge) not possible to logon as Pico8 without using administrative Rights (such as sudo or su). Also SSH as User "Pico8" is not possible (unless you change something on the SSH-Server).


## I wanna avoid the blinking Cursor on boot
add `vt.global_cursor_default=0` on your `cmdline.txt`. I think it's OK to show the blinking Cursor on Boot. At least you can see that the Device is working^^.


## Can I install the GPi Case Shutdown-Script?
I _think_ it's possible and I also _think_ it'll work; but I didn't installed the Script.


# Known Issues
* Performance-Issues. Some Complex Cards may slowing down the Performance, maybe some Overclocking could help (use with caution!).
* No Escape-Key. Normally, that's not a Problem as long as you have at least one Game (either on BBS or locally on your "carts"-Folder), because for some weird reason, you cannot shutdown PICO8 without a Highlighted Cart. Sounds weird, and it is weird^^.
* If you click "load more" multiple times, PICO8 freezes. I don't think I can change anything about that^^.
