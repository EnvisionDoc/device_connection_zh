# Unit 1: Setting up the Raspberry Pi

In this tutorial, set up a Raspberry Pi 3 Model B as the main device. The RPi can connect to a set of sensors and small plug-ins.

## Hardware Preparation

To set up the RPi for this lab, the following hardwares are needed:

- A Raspberry Pi 3 Model B (with power supply)
- A 32GB TF card (with reader)
- An Ethernet cable
- A USB flash disk
- An LED light
- Several RPi DuPont cables
- A USB mouse
- A USB keyboard
- An HDMI cable
- A monitor with HDMI input

You can also choose other RPi plug-ins according to your needs.

Now you can set up the RPi with the selected hardwares. For detailed steps, refer to the [Raspberry Pi User Guide](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up) documentation.

## Setting Up Raspberry Pi

With the hardware set up, you can now start up the Raspberry Pi and take the following steps to install the needed software.

1. Download the official operating system file `Raspbian` for the RPi at https://www.raspberrypi.org/downloads/.

2. Make a system card for the RPi. For detailed steps, refer to the readme file at https://www.raspberrypi.org/documentation/installation/installing-images/README.md.

3. Insert the system card into the RPi and power it on. For a normal running status, the red light on the RPi will be on, and the green light will be flickering. The monitor will be displaying the following desktop of the RPi:

   .. image:: media/desktop.png

4. Connect the sensors and LED light to the RPi, like below:

   .. image:: media/sensors.png

After the hardware of the RPi is set up, you can then register the devices on EnOS Console.

## Next Unit

[Registering the Raspberry Pi on EnOS Console](registering_devices)

<!--end-->