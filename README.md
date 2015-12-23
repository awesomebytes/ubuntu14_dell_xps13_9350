# Install Ubuntu 14.04 on XPS 13 9350 with Broadcom 4350 card with kernel 4.4-rc6

Wifi works, bluetooth works, suspend works, audio works (with headphones too), battery life reports 7h on 100% (haven't tested it enough).

## Prepare BIOS

Go to the BIOS (press F12 on boot) > BIOS Setup

    On General > Boot Sequence > Boot List Option
    I have UEFI

    On System Configuration > SATA Operation
    AHCI

This is necessary or ubuntu won't recognize the NVME disk

    On Secure Boot > Secure Boot Enable
    Disabled

    POST Behaviour > FastBoot
    Thorough

## Get the latest Ubuntu 14.04 Live ISO and burn it to a pendrive (I use unetbootin)
Official ISO Image: [http://www.ubuntu.com/download/desktop](http://www.ubuntu.com/download/desktop)
Unetbootin: [https://unetbootin.github.io/](https://unetbootin.github.io/)

## Copy my prepared package to the pendrive (anywhere)
[https://github.com/awesomebytes/ubuntu14_dell_xps13_9350/raw/master/kernel_v4.4-rc6_with_wifi.tar.gz](https://github.com/awesomebytes/ubuntu14_dell_xps13_9350/raw/master/kernel_v4.4-rc6_with_wifi.tar.gz)
You can also download this repo to have the instructions downloaded too:
[https://github.com/awesomebytes/ubuntu14_dell_xps13_9350/archive/master.zip](https://github.com/awesomebytes/ubuntu14_dell_xps13_9350/archive/master.zip)

It has gparted 0.24 that you will need in the live cd to be able to install the system.
It also has kernel 4.4-rc6 and the bcm driver and a script to install all that.

## Boot the Live CD 
Press F12 on boot and Choose:

UEFI BOOT > UEFI: YOURPENDRIVE BRAND

In my case:

UEFI BOOT > UEFI: KingstonDT microDuo 3.0 PMAP, Partition 1

(Note that a SanDisk pendrive always failed to boot the ubuntu livecd for some reason)
Note that the livecd sometimes didn't boot at the first try (black screen) there is some problem with the graphic driver.

# Install new Gparted to enable NVME disk installing
Open a shell (control+alt+T)

    cd /cdrom
    cp kernel_v4.4-rc6_with_wifi.tar.gz /home/ubuntu
    cd /home/ubuntu
    tar xvfz kernel_v4.4-rc6_with_wifi.tar.gz
    cd kernel_v4.4-rc6_with_wifi
    sudo dpkg -i gparted_0.24.0-1-getdeb1_amd64.deb

# Install Ubuntu 14.04 LTS
Click on the Install Ubuntu 14.04.03 LTS icon.

Configure as you like BUT DON'T ENABLE DOWNLOAD UPDATES WHILE INSTALLING NOR INSTALL THIRD PARTY SOFTWARE.
It will freeze your installation. If you don't believe me, just try and happy reboot.

I Shrinked the Windows 10 partition on Windows 10 (Going to my computer > right click > Manage > Disk Manager, choose to shrink, I left it to 100GB).
It is very recommendable that you do that from there, don't resize from Gparted. If you changed to AHCI SATA mode, you'll need to change back to RAID to boot Windows 10 to do this. Sorry!

I gave it 100GB for / and 200GB for /home, 100GB extra for testing pourposes as ext4 and the rest, 8GB, for swap.

If when continuing you get an error of choosing a boot partition, you didn't boot in UEFI Mode, you used Legacy. Bad doggy. Read again my instructions.

From a USB 3.0 (on the right port of the laptop, it shouldn't matter) to the SSD disk it took approximately 3 minutes to install the system.

# Boot on your new system and install kernel 4.4-rc6
Open a terminal (control+alt+t)
Go to your pendrive

    cd /media/<youruser>/<yourpendrivename>
    cp kernel_v4.4-rc6_with_wifi.tar.gz ~
    tar xvfz kernel_v4.4-rc6_with_wifi.tar.gz
    cd kernel_v4.4-rc6_with_wifi
    chmod +x install_kernel4.4_with_wifi_and_nvme.bash
    ./install_kernel4.4_with_wifi_and_nvme.bash


Reboot (maybe twice, first time the graphics sometimes don't work, dunno why) and you have wifi.

Sometimes there are graphical artifacts, but after installing gnome-session-flashback (I don't like unity)
they don't appear anymore (login with Metacity).


To gain back windows access without changing the SATA mode every time I found this link:
http://www.tenforums.com/performance-maintenance/15006-attn-ssd-owners-enabling-ahci-mode-after-windows-10-installation.html

Which I haven't tested yet.




