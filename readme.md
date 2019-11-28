# Raspberry Pi motion-capture camera trap for very small creatures

## What?

[Mike Whitehead](https://michaelwhitehead.net/) (mostly) and [me](http://robertlanfear.com/) (a bit) are interested in using camera traps to make observations about insect pollinators and other things in Australia. Initial trials with the camera have successfully shown that a native Australian heath is [both visited by birds, and unable to be accessed by honeybees](https://youtu.be/fvPg0uDEknc), and under the right conditions the camera will trigger for [insects as small as a mosquito.](https://youtu.be/W0o_thmN8pM) 

As well as capturing ecological data on insects in the field, this camera trap design could be very useful in a lab or controlled environment where researchers want to record movement of their study animals. For example, it could be mounted looking down on arenas/containers of beetles or crickets, or placed at the entrance of tubes or burrows housing small lizards.  

This repo is an attempt to keep things in order, and keep notes of how to build, set up, and use the camera traps. 

If using this guide, remember, it is only a guide, and merely reflects what we have done to get these up and running for our specific needs. This is a vast space of DIY options, and almost every step can be done in another way, with another program, or a different bit of hardware. If you have ideas to clearly improve on what we've done, please contribute! 

## Setup

#### 1. Buy what you need

Here's what you need, and at least one place you can get each thing. Prices were accurate in mid 2019. 

For each camera trap:

Thing | Price | Link
------------ | ------------- | -------------
Raspberry Pi 3 B | $57 | [link](https://www.littlebird.com.au/collections/raspberry-pi/products/raspberry-pi-3-model-b-f1990217-84ed-4cd4-a75a-bd1962465bd8)
Raspberry Pi camera V2 | $40 | [link](https://www.littlebird.com.au/products/raspberry-pi-camera-board-v2)
SanDisk 64GB microSDXC | $22 | [link](https://www.officeworks.com.au/shop/officeworks/p/sandisk-ultra-64gb-micro-sdxc-memory-card-sdsq64gb)
Anker PowerCore+ 10050mAh QC 2.0 * | $69 | [link](http://www.buzzgadgets.com.au/anker-powercore-plus-10050mah-portable-battery-charger-black.html?gclid=Cj0KCQjwg7HPBRDUARIsAMeR_0g6MQOVX5g0RElIYtw4LQTRVpH7G-pLAVSdR3GcaE7i3R-tefYY7_kaAtf1EALw_wcB)

Grand total: $188 (excluding shipping)

*Apparently these have become difficult to source. But any powerbank with a USB output will work. When buying, keep in mind size and shape, as this determines what size container you can fit it in. 

You'll also need a single portable wireless router to help with using these in the field. If you are using them in range of your home wireless netowrk, this is not necessary.

Thing | Price | Link
------------ | ------------- | -------------
TL-WR802N Wireless N Nano Router| $29.00 | [link](https://www.mwave.com.au/product/tplink-tlwr802n-300mbps-wireless-n-nano-router-ab64161?gclid=Cj0KCQjwg7HPBRDUARIsAMeR_0hcFvUOAFaheZYIzXz1Pkn0HBXFMVBssw69OmRT5FxTK9j_ZKP8kwIaAkWKEALw_wcB)


You can buy various cases for the raspberry pi. They are not strictly necessary but they can be useful for some situations. 

Other things you'll need:

1. A laptop

2. A microSD card reader that connects to your laptop (if your laptop doesn't have an SD card reader already)

3. Internet access to get things set up

4. A medium-sized plastic food storage container to make the whole thing waterproof. A case 150 x 100 x 60 is just sufficient to house all components.

5. A drill bit. 6mm diamond core is recommended, but other ones will work.

If setting up multiple camera traps, we recommend labelling components individually. At the minimum, labelling each pi and microSD card is very useful. 




#### 2. Charge your battery/batteries

You'll need a USB port to charge each battery. Any USB port works fine, including those on your laptop, or for phone chargers etc. They take a few hours to charge.



#### 3. Set up microSD card

1. Format your SD card for FAT (MS-DOS). Protocol here depends on your computer. 

2. Download raspbian to your computer: https://downloads.raspberrypi.org/raspbian_latest

3. Unzip the raspbian image that you just downloaded

4. Attach your microSD to your computer

5. Install raspbian onto the memory card. Full instructions, covering lots of approaches on all operating systems, are here: https://www.raspberrypi.org/documentation/installation/installing-images/README.md

If you are comfortable using the commandline, and you are using a mac, here are the instructions you can follow. Don't just copy-paste the code below if you're not sure what it does. Some of the code alters disks, and if you get it wrong you could be altering your hard drive not the SD card.

```shell
# figure out which disk is the SD card. In this example we'll assume it's `disk2`, but the list will show you what is what
diskutil list

# unmount the SD card. Change 'disk2' to whatever the SD card is on your system
diskutil unmountDisk /dev/disk2

# copy the disk image over - change the location of the disk image of course..., and change 'disk2'
sudo dd bs=1m if=/Users/roblanfear/Desktop/2017-11-29-raspbian-stretch.img of=/dev/rdisk2 conv=sync
```

6. Copy and paste the `ssh` file in this repository (it's just an empty text file named 'ssh') onto the SD card. This lets your raspberry pi work in headless mode (i.e. with no screen). More info here: https://stackoverflow.com/questions/41318597/ssh-connection-refused-on-raspberry-pi

You can do this using your normal file browser, or at the commandline something like this:

```shell

# now it has the disk image on it, you should be able to access the SD via /Volumes/boot
sudo cp /Users/roblanfear/Documents/github/rasberry_trap/ssh /Volumes/boot

```

#### 4. Build the Rpi

1. Put the pi board in the case, if you have a case. Your case will depend on your needs, and what you have available. It is not strictly necessary. Our own build is detailed at the end of these instructions. 

2. Attach the camera, following the instructions included with the camera board

3. Finish off the case

4. Insert the memory card

5. Insert the wireless adaptor into one of the USB slots. Your raspberry pi might look something like this.



#### 5. Set up your RPi in headless mode (you will need ethernet access to the internet for this)

1. Connect the raspberry pi to power (you can use your battery if it's charged)

2. Connect your raspberry pi by an ethernet cable to your usual router (i.e. one that is connected to the internet - we'll use the tp link router later). 

3. Follow the commands below, using the commandline on your laptop (e.g. terminal on a mac). More details of these instructions are available here, starting at step 2, and ending when you've done step 5: https://www.raspberrypi.org/forums/viewtopic.php?t=74176. 

```shell

# find for pi's IP address
# the IP is the ```10.0.0.6``` on the line that looks like this: ```raspberrypi.gateway (10.0.0.6)```
arp -a

# ssh to it, default password is 'raspberry'
# your IP will be different, get it from the step above
ssh pi@10.0.0.6

# if the raspi config screen doesn't come up on its own...
sudo raspi-config
```

In the configuration screen, do these things:

* Change the password (option 1)

* Expand the file system (option 7 then option A1)

* Enable the camera (option 5 then option P1)

* Change the name (option 2 then option N1)

* Set the timezone (option 4 then option I2)

* Finally, select `<finish>` and then reboot

Changing the pi's name is useful if you plan on having lots of camera traps. You should be able to connect to your pi by name, e.g. if you named it `trap2`, then at your laptop's commandline:

```shell
ssh pi@trap2
```



4. Update and upgrade everything, the second step takes ~5 minutes

```
sudo apt-get update
sudo apt-get upgrade
```


#### 6. Install motion-capture software 

Keep your pi connected to the ethernet for this step. SSH into your raspberry pi, then:

```shell

cd ~

# get the software from github
git clone https://github.com/billw2/pikrellcam.git

# You will be asked a few questions...
# web port is usually 80
# we say 'yes' to starting at boot
# we don't set up passwords for field use. For home/city use it might be a good idea.
cd pikrellcam
./install-pikrellcam.sh
```

Now you should be able to control the motion capture software from your browser by logging into `http://trap2` where 'trap2' is whatever you've named your pi. Full details are here: http://billw2.github.io/pikrellcam/pikrellcam.html. If you cannot connect via the browser, you will need to troubleshoot the installation of the motion capture software.




#### 7. Set up wifi. 

More details on the commands below are here: https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md. If the commands below don't work for you, please visit that link and read through carefully. Below is just a simplified version of what's at that link.

For convenience, I add the network for my tp link router as `priority=1`, and my home/work network as `priority=2`. This way, if both networks are present then you can connect to the tp link router to test your field setup while still at home.

To do that, first open up the file you need to edit, by connecting to your pi then typing:

```shell
sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
```

Then add this text to the end of the file, changing the text to match your network names and passwords (the details for the tp link router are on the small sticker on the back; the password is the ~8 digit number):

```
network={
    ssid="TP-LINK_CE99"
    psk="tplinkpassword"
    priority=1
    id_str="field"
}

network={
    ssid="homenetwork"
    psk="homepassword"
    priority=2
    id_str="home"
}
```

Quit nano with `ctrl+x` then `Y` to save the settings, and `ENTER`.

Once this is done, reconfigure the network settings and reboot with:

```shell
wpa_cli -i wlan0 reconfigure
```

Finally, reboot:

```shell
sudo reboot
```


#### 7. Check your setup

Now we want to check that the tp link router will work with the raspberry pi.

1. Disconnect the pi from the ethernet and the power

2. Plug the tp link nano router into the power (this should create the network you'll have in the field)

3. Connect the pi to the power

4. Connect your laptop to the network created by your tp link router, e.g. `TP-LINK_CD99`

6. 


#### 8. Configure motion-capture software

PiKrellCam comes with loads of options to configure zones of detection and the thresholds for motion detection. We recommend reading the documentation ***Insert instructions on where to find detailed help on configureation, I think it is actually in the PiKrellCam program, as I can't see it online?***

A quick explanation for what some of the configuration options refer to:
Mag: The degree (magnitude) of movement threshold
Cnt: The threshold number (count) of pixels that are moving
Preview_cleaning: Leave motion parameters embedded on image, or clean them off
Confirm_gap: Require another movement within x seconds of previous detection before firing

To access pikrellcam.conf in order to manually change settings without webbrowser, 

$ cd .pikrellcam 
$ sudo nano pikrellcam.conf

***Insert settings I am using***




#### 9. Build the camera trap

1. Drill a hole for your camera in the lid of the container. The a 6mm diamond core drill bit will drill a hole *slightly* too small for the Pi Camera lens element. So after drilling the initial hole for the lens, gently wiggle the outside of the spinning drill bit to slightly expand the hole. You should aim to sculpt out a circle only just large enough for the lens, such that it can be snapped into place and held by the plastic. So expand slightly, test with the Pi Camera, and drill out larger if necessary.

2. The Pi Camera is simple to install. Lift the black tabs on the camera port (in between HDMI and AV) and insert the camera board ribbon with contacts facing the HDMI port. Now snap the black tabs back down to secure. 

3. Snap the camera into the drilled hole. Secure with some gaffer tape (or similar) to hold the camera against the inside of the container. 

4. To complete the unit, place the battery with cable into the container, and snap the lid shut. 


There is no limit to altnerative options for custom housing and securing your Raspberry Pi. For example, foam padding could be employed if you expected the Pi might need shock absorbers, painting could be done for crytpic coloration, a container with ventilation would be better in conditions where one did not expect it to get wet (e.g. lab experiment), and off-the-shelf Raspberry Pi housings could be used if the unit is to be powered by another kind of power source. 




#### 10. Adjusting the camera's optics

The Pi camera has a native minimal focal length (~50cm) that is too long to capture action close to camera. The focal length can be customized by rotating the lens element in its plastic housing. The idea is to increase the distance bwteeen the lens element and the sensor, thereby bringing the focal distance closer to the camera.  

1. Connect your camera to your Pi, and connect your Pi to the local network, or to a monitor.

2. Open PiKrellCam software (follow instructions above), or run any camera application that will provide you with a live view of the camera's view. 

3. Looking at the lens, hold the camera board such that the ribbon is aimed down. An anticlockwise rotation will shift the focal point inwards towards the lens, clockwise rotation will move the focal point out towards infinity. 

4. Using a pair of tweezers, turn the lens a small fraction of a turn (e.g. 10 degrees). Using a tape measure, or a ruler, in the live view, finely adjust by small movements to manipulate the focal length. 

I have found a focal point of 20 - 30 cm from the lens element gives a field of view roughly the size of an A4/Letter piece of paper, and this works well for small insects. 



#### 11. Setting date and time on the RPi (optional)

Because your Pi is not going have consistent access to power or the Internet, the date and time often needs to be set manually. This can be done via SSH. 

1. Open a terminal window

2. SSH into the pi unit by typing: 
 > ssh pi@XXX.XXX.X.X 

Where X is the IP address of the Pi Unit. 

3.	Type ‘yes’ if you get an authenticity warning.

4. Enter the password

5.	Change the time by typing
 > sudo date -s '2017-11-16 11:21:00'

6. Change the timezone by typing  
 > sudo dpkg-reconfigure tzdata




### Using it in the field

Once set up, the camera is easy to run in the field. 

1. Turn on your portable Wi-Fi network, and connect your laptop/tablet to the network. 

2. Plug the power supply in to your Pi. It should automatically connect to the WiFi network and boot up PiKrellCam. 

3. In a browser, type in the IP address for the Pi unit. Enter login and password if prompted. 

4. You should see the PiKrellCam interface. Click START. (This can be a bit tricky if the interface is flicking [don't know what causes this], as the button moves button around). On clicking START, you have just initiated the PiKrellCam motion script. You can now control the camera recording, the program’s configuration and view/delete videos. 

5. The camera is now running motion capture. You will want to switch it off straight away. Click the button ENABLE: MOTION. This will stop motion from being active. 

6. Position the camera where you like, using the live view to frame the shot. 

7. Secure the camera. I use cam-straps to fasten to a wooden stake. But one could use tripods, beanbags, cable-ties, whatever works.

8. Grooming the scene can drastically reduce false positives! Remove thin, waving blades of grass, or carefully balanced dry leaves from the field of view. The cleaner the scene from lightweight waving objects, the better the results. 

9. If required, set date and time via SSH (see separate instructions for this [LINK]. 

10. When ready to begin motion capture, click ENABLE: MOTION to start running once more.

Once running, the Wifi network can be turned off and the camera will continue to run the PiKrellCam motion detection. 


