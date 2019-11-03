# Headless-Kali-Pi
This is for my project on setting up a headless-kali-pi.

My main source of inspiration, but I was having problems and needed headless.

https://null-byte.wonderhowto.com/how-to/build-beginner-hacking-kit-with-raspberry-pi-3-model-b-0184144/

This was built from a combination of info.

## You will need:
- A network
- Rasberry Pi and its contents like power supply
- SD micro
- Another linux system - I recomend a live OS if you have windows.
- Image program - you can use [win32diskimager](https://sourceforge.net/projects/win32diskimager/) , [ApplePiBaker](https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/) or [Etcher](https://www.balena.io/etcher/) 
- The kali-pi image [kali-pi image](https://whitedome.com.au/re4son/sticky-fingers-kali-pi-pre-installed-image/)

At this point I trust you can use one of the software to flash the kali-pi image to your SD micro.

## Make Headless
Now to make headless.
In another linux plug in the device and go to mount the bigger partion.
You can do this using the GUI or commands. [Here is a little guide for to mount through terminal if you want](https://linuxconfig.org/howto-mount-usb-drive-in-linux)
I prefer GUI


### more info on mounting (optional)
 - [mount examples](https://www.thegeekstuff.com/2013/01/mount-umount-examples/?utm_source=tuicool)
 - [mount all example, etc](https://linuxize.com/post/how-to-mount-and-unmount-file-systems-in-linux/)
 - [Set up automatically mount](https://unix.stackexchange.com/questions/134797/how-to-automatically-mount-an-usb-device-on-plugin-time-on-an-already-running-sy)

After mounting make sure to remember the files path and go to the terminal 
and type 

```
cd /media/[device name here]/etc/network/
```
You will need to fill in the details `[device name here]` necessary.


I have videos that have helped me throw this [raspberry pi headless](https://core-electronics.com.au/tutorials/raspberry-pi-zerow-headless-wifi-setup.html) and [kali linux headless](https://www.youtube.com/watch?v=4SeVEWXkW30) (Caution second one was not that great)


### File interfaces

Then type.
```
sudo chmod 766 interfaces
sudo nano interfaces
```
In the file copy and paste this and press ctrl-x, then y to save, followed by an enter.
```
auto lo
iface lo inet loopback

auto wlan0
iface wlan0 inet dhcp
wpa-conf /etc/wpa_supplicant/wpa_supplicant.conf

allow-hotplug usb0
iface usb0 inet dhcp
```
### File wpa_supplicant.conf

Now that you are out of that file next is to.
```
cd /media/[device name here]/etc/wpa_supplicant/
sudo chmod 766 wpa_supplicant.conf
sudo nano wpa_supplicant.conf
```
Copy and paste, but with changes to `MyWiFiNetwork` to your wifi name and `aVeryStrongPassword` to the password.
To exit out press ctrl-x, then y to save, followed by an enter.
```
country=AU
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
network={
ssid="MyWiFiNetwork"
psk="aVeryStrongPassword"
key_mgmt=WPA-PSK
}
```
### File ssh
Ok mount the smaller partion
In the main dir create a file named ssh.
```
cd /media/[device partion 2 name here]
echo "" > ssh
```

## Set Up Auto Login
This if for incase you ever want to boot up the device with a screen. [video](https://www.youtube.com/watch?v=U5UkLPb7f8w)
###  File lightdm.conf
```
cd /media/[device name here]/etc/lightdm/
sudo nano lightdm.conf
```
Comments would be with the '#'

Remove comment 
- `autologin-user=root`
- `autologin-user-timeout=0`

Save and exit out of nano
- Press `Ctrl + X`
- Press `Y` for yes to Save
- Press `Enter` to keep the current name

###  File lightdm-autologin
```
cd /media/[device name here]/etc/pam.d/
sudo nano lightdm-autologin
```
Comment if exsists
- `auth required pam_if.so user != root quiet_success`

Save and exit out of nano
- Press `Ctrl + X`
- Press `Y` for yes to Save
- Press `Enter` to keep the current name



You can now unmount and remove the SD out of the linux system to now plug it in to the Raspberry pi.
If everything is done right, you are ready to boot your Raspberry pi for the first time. :)

The idea here is to connect to the Raspberry pi through SSH.
On boot up you should see the Raspberry pi on you network.

There are a lot of diffrent ways to see it on your network.
I recommend using an app on your phone called fing [android](https://play.google.com/store/apps/details?id=com.overlook.android.fing&hl=en) or [iphone](https://apps.apple.com/us/app/fing-network-scanner/id430921107) or [windows pc](https://www.advanced-ip-scanner.com/)

There after scanning you will see a list of IP's
Grab the numbers of the one under Raspberry Pi. It should look like ###.###.###.###

### Now to use the SSH
(On Mac or linux terminal) `ssh root@[IP goes here]`

(On Windows) Download and install
[Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- Ip or host is the one you just grabbed. 
- user is root
- password is toor

If you need help this is not exact but should be a good [example](https://www.youtube.com/watch?t=325&v=LlCr09B2HZI)

When in congrats you are now headless.

You can take it a step further



### Set Up Remote - x11vnc

In the SSH terminal
```
apt-get install x11vnc
x11vnc -storepasswd
```
enter password you want

To run it 
```
x11vnc -usepw -reopen -bg -forever &
```

If the you are having problems like [screen size fix](https://dephace.com/change-screen-resolution-in-kali-linux-on-raspberry-pi-3/) or [start up](https://unix.stackexchange.com/questions/276463/how-to-execute-shell-script-on-kali-linux-startup), [etc..](https://raspberrypi.stackexchange.com/questions/60874/tightvncserver-displaying-grey-screen-on-kali-linux-upon-vnc-connection)


## Update Everything - Needs to be done on remote or Not Headless

Personal prefrence to have an on screen keybord (Can ignore)
```
sudo apt-get install matchbox-keyboard
```

You will need to expand the space 
```
apt-get install gparted
gparted
```
Now use the gparted GUI to resize and comit the changes to the partition.

Then
```
apt-get update && apt-get upgrade
```


## Things to be more secure from bots.

###!!IMPORTANT!!
change all users password.

This is for the current user
```
passwd
```

That is for the Pi user
```
passwd pi
```

- https://hub.packtpub.com/how-to-secure-your-raspberry-pi-board-tutorial/
- https://linuxhandbook.com/fail2ban-basic/
