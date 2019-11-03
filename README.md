# Headless-Kali-Pi
This is for my project on setting up a headless-kali-pi.

My main source of inspiration, but I was having problems and needed headless.

https://null-byte.wonderhowto.com/how-to/build-beginner-hacking-kit-with-raspberry-pi-3-model-b-0184144/

This was built from a combination of info.

## You will need:
- A network
- Rasberry Pi
- Another linux system - I recomend a live OS if you have windows.
- Image program - you can use [win32diskimager](https://sourceforge.net/projects/win32diskimager/) , [ApplePiBaker](https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/) or [Etcher](https://www.balena.io/etcher/) 
- The kali-pi image [kali-pi image](https://whitedome.com.au/re4son/sticky-fingers-kali-pi-pre-installed-image/)

At this point I trust you can use one of the software to flash the kali-pi image to your SD micro.

## Make Headless
Now to make headless.
In another linux plug in the device and go to mount the bigger partion.
You can do this using the GUI or commands. [Here is a little guide for to mount through terminal if you want](https://linuxconfig.org/howto-mount-usb-drive-in-linux)
I prefer GUI


### more info on mounting totaly optional
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
cd /etc/wpa_supplicant/
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
Ok mount the smaller partion
In the main dir create a file named ssh.
