# Catching insects with raspberries

## What?

[Mike Whitehead](https://michaelrwhitehead.wordpress.com/) (mostly) and [me](http://robertlanfear.com/) (a bit) are interested in using camera traps to make observations about pollinators and other things in Australia. This repo is an attempt to keep things in order, and keep notes of how to build, set up, and use the camera traps. 

## Shopping list

Here's what you need, and at least one place you can get each thing. Prices were accurate in late 2017. 

Thing | Price | Link
------------ | ------------- | -------------
Raspberry Pi 3 B | $57.95 | [link](https://www.littlebirdelectronics.com.au/raspberry-pi-3~38194)
Raspberry Pi camera V2 | $59.90 | [link](https://www.littlebirdelectronics.com.au/raspberry-pi-camera-board-v2-8-megapixels)
SanDisk 64GB microSDXC | $47.98 | [link](https://www.mwave.com.au/product/sandisk-64gb-extreme-microsdxc-uhsi-card-45mbs-ab52673?gclid=Cj0KCQjwg7HPBRDUARIsAMeR_0hD-Axpejh8nX-CrpZ7bEYesIIegPrurTqMBcV850F9jfABD0bgnBIaAoJxEALw_wcB)
Anker PowerCore+ 10050mAh QC 2.0 | $59.00 | [link](http://www.buzzgadgets.com.au/anker-powercore-plus-10050mah-portable-battery-charger-black.html?gclid=Cj0KCQjwg7HPBRDUARIsAMeR_0g6MQOVX5g0RElIYtw4LQTRVpH7G-pLAVSdR3GcaE7i3R-tefYY7_kaAtf1EALw_wcB)
TL-WR802N Wireless N Nano Router| $29.00 | [link](https://www.mwave.com.au/product/tplink-tlwr802n-300mbps-wireless-n-nano-router-ab64161?gclid=Cj0KCQjwg7HPBRDUARIsAMeR_0hcFvUOAFaheZYIzXz1Pkn0HBXFMVBssw69OmRT5FxTK9j_ZKP8kwIaAkWKEALw_wcB)

Grand total: $253.83 (excluding shipping)

Note that the Router is just to aid in setting up the camera trap in the field. You'll also need a laptop, which isn't included here. One could set these things up without a router, e.g. by programming them at home to start the camera trap when they turn on, or by using a small screen and keyboard plugged straight into the raspberry pi. In general though, it helps to have a router and a laptop, because other options make maintenance a pain.

Other things you'll need:

1. A laptop
2. A microSD card reader that connects to your laptop
3. Internet access to get things set up
4. A medium-sized plastic food storage container to make the whole thing waterproof. A case 150 x 100 x 60 is just sufficient to house all components.
5. A drill bit. 6mm diamond core is recommended, but other ones will work.

## Building it

1. If setting up multiple units label components individually. At the minimum, labelling each Pi and microSD card is essential. 

2. Drill a hole for your camera in the lid of the container. The a 6mm diamond core drill bit will drill a hole *slightly* too small for the Pi Camera lens element. So after drilling the initial hole for the lens, gently wiggle the outside of the spinning drill bit to slightly expand the hole. You should aim to sculpt out a circle only just large enough for the lens, such that it can be snapped into place and held by the plastic. So expand slightly, test with the Pi Camera, and drill out larger if necessary.

3. The Pi Camera is simple to install. Lift the black tabs on the camera port (in between HDMI and AV) and insert the camera board ribbon with contacts facing the HDMI port. Now snap the black tabs back down to secure. 

4. Snap the camera into the drilled hole. Secure with some gaffer tape (or similar) to hold the camera against the inside of the container. 

5. To complete the unit, place the battery with cable into the container, and snap the lid shut. 


There is no limit to altnerative options for custom housing and securing your Raspberry Pi. For example, foam padding could be employed if you expected the Pi might need shock absorbers, painting could be done for crytpic coloration, a container with ventilation would be better in conditions where one did not expect it to get wet (e.g. lab experiment), and off-the-shelf Raspberry Pi housings could be used if the unit is to be powered by another kind of power source. 



## Setting it up

#### 1. Put raspbian on the memory card
#### 2. Set up your RPi in headless mode
#### 3. Install motion-capture software
#### 4. Configure motion-capture software

## Using it in the field

