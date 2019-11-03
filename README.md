# headless-kali-pi
This is for my project on setting up a headless-kali-pi.

My main source of inspiration, but I was having problems and needed headless.

https://null-byte.wonderhowto.com/how-to/build-beginner-hacking-kit-with-raspberry-pi-3-model-b-0184144/

This was built from a combination of info.

## You will need:
- A network
- Rasberry Pi
- Another linux system - I recomend a live OS if you have windows.
- Image program - you can use win32diskimager, ApplePiBaker or Etcher 
- The kali-pi image

## Download a image program

- https://sourceforge.net/projects/win32diskimager/
- https://www.tweaking4all.com/software/macosx-software/macosx-apple-pi-baker/
- https://www.balena.io/etcher/

## Download kali-pi image

- https://whitedome.com.au/re4son/sticky-fingers-kali-pi-pre-installed-image/


At this point I trust you can use one of the software to flash the kali-pi image to your SD micro.

## Make Headless
Now to make headless.
In another linux plug in the device and go to mount the bigger partion.
You can do this using the GUI or commands.
I prefer GUI

If you have want to do it through terminal and have trouble here is a little guide for to mount.
- https://linuxconfig.org/howto-mount-usb-drive-in-linux
And here is more info on mount if you are interested, totaly optional
 - https://www.thegeekstuff.com/2013/01/mount-umount-examples/?utm_source=tuicool
 - https://linuxize.com/post/how-to-mount-and-unmount-file-systems-in-linux/
 - https://unix.stackexchange.com/questions/134797/how-to-automatically-mount-an-usb-device-on-plugin-time-on-an-already-running-sy

After mounting make sure to remeber the files path and go to the terminal 
and type 
cd /media/[device name here]/etc/network/
You will need to fill in the details were necessary.
