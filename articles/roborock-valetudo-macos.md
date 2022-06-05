# Install Valetudo on Roborock S5 via MacOS

In this article, I want to show you how you can install the custom firmware [Valetudo](https://valetudo.cloud/) on your [Roborock S5 (Max)](https://de.roborock.com/pages/roborock-s5-max) using your Mac.

Usually, when you buy a Roborock S5, it is expected that you use the Xiaomi Home App. However, this app sends all its data (including your floor data or Wi-Fi passwords!) to the cloud and servers located in China. When you are uncomfortable with this (like I am), then there is Valetudo to the rescue. Basically, Valetudo is a custom firmware that you can flash on your robot vacuum (not just the Roborock, there are [many vacuums supported](https://valetudo.cloud/pages/general/supported-robots.html)) which then stops the robot from sending its data to the cloud. Instead, it runs a web server from which you can control the robot.

*DISCLAIMER*

The following steps must be performed at your own risk. They can potentially brick your device and I will not take responsibility for that. If unsure, have a look at the [Valetudo Rooting Instruction](https://valetudo.cloud/pages/general/rooting-instructions.html) on what you should be comfortable with before rooting your device.

## Create the firmware image

Fortunately for us, there is a wonderful project that you can use to create a firmware image for your robot vacuum called [Dustbuilder](https://builder.dontvacuum.me/). Simply navigate to the website, choose your robot and fill out the form. You can leave almost all fields as is, just enter your email address, select if you want to have an SSH key pair generated or simply upload an existing public key.

Next, select the option *Build update package (for installation with python-miio/RRCC)* and choose the version *S5 (ver 2008, 12/2019, stripped-Ubuntu) update to this version first, if your current is < 2008*.

Finally, complete the form and wait for Dustbuilder to create the firmware image for you. If it is ready to download, you will get notified via the email address you entered before.

## Prepare the robot

We now reset our robot completely in order to prepare it for flashing our custom firmware. For this, perform the following steps:

1. Press the recharge button for 3-5 seconds
2. Continue pressing the recharge button and briefly press the reset pin next to the Wi-Fi LED once. Now all button LEDs should go off.
3. Keep the recharge button pressed until the lights come back on and you hear a voice stating that the robot will be reset to its original state. Now release the recharge button.
4. Wait until the button LEDs on the robot continuously glow again. Now the robot is reset and you should see its WIFI network again.

## Flash the firmware image

When your firmware image is ready to download and the robot is reset to its original state, we can finally flash Valetudo. For this, perform the following steps (`python3` must be installed on your Mac):

1. Upgrade pip: `python3 -m pip install --upgrade pip`
2. Install the *python-miio* package to communicate with our robot: `pip3 install python-miio`
3. Add the `mirobo` binary to your PATH
4. Retrieve the token from your robot: `mirobo -d discover --handshake true`. Look in the output for the token (32char length)
5. Flash the firmware (just retry command, sometimes doesn't work on the first tries): `mirobo -d --token <TOKEN> --ip 192.168.8.1 update-firmware <FIRMWARE_PKG_FILE>`

Now wait for the flashing to complete, this will take some time but you should see the progress in your terminal. If it is finished, the LEDs on your robot will start to blink. Wait for them to continuously glow again, the robot should then state that the update has successfully finished.

Connect to the Wi-Fi of the robot again and open http://192.168.8.1 in your Web Browser. You should see a page prompting you to enter the Wi-Fi credentials your robot should connect to. Fill out the form and connect to your regular Wi-Fi again. From now on, Valetudo on your robot can be accessed via the IP your router assigned it.

This is it. I hope you managed to set up Valetudo on your robot vacuum and everything works as expected :)