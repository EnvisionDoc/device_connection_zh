# Unit 2: Registering the Raspberry Pi on EnOS Console

Before connecting the RPi into EnOS, you need to register the RPi device on the EnOS Console, including defining a model, product, and device.

## Step 1: Creating a Model for RPi

In this step, we’ll create a model name *RPi* with the needed attributes. Detailed steps are as follows:

1. In the EnOS Console, click **Model** from the left navigation panel.

2. Click **New Model**, provide the following basic settings for the model, and click **OK**.

   .. image:: media/create_model.png

3. From the model list, click **Edit** for the *RPi* model, and click **Feature Definition** to define the following measuring points:

   .. csv-table::
      :widths: auto

      "Measuring Point", "Description"
      "*Temperature*", "AI type data collected from the temperature sensor."
      "*Humidity*", "AI type data collected from the humidity sensor."
      "*Light*", "DI type of data sending to Raspberry Pi to control the light. Sending 0 stands for light off, sending 1 stands for light on, and sending 2 stands for light flickering."
      "*Light_Flicker*", "DI type of data collected from Raspberry Pi to show the light status. Getting 0 stands for light off, and getting 1 stands for light on."


See the following screen capture of the created measuring points:

.. image:: media/measuring_points.png

You can define your own measuring points in your device model based on your needs.



## Step 2: Creating a Product

A product is a collection of devices with the same features. In this tutorial, create a product named *RPi_Product* under the *RPi* model. Detailed steps are as follows:

1. In the EnOS Console, click **Device Management** > **Product** from the left navigation panel.

2. Click **New Product**, provide the following basic settings for the product, and click **OK**.

   .. image:: media/create_product.png



## Step 3: Registering the Device

In this step, create a device named *RPi_Device*, which belongs to the product created in the previous step. Detailed steps are as follows:

1. In the EnOS Console, click **Device Management** > **Device** from the left navigation panel.

2. Click **New Device**, provide the following basic settings for the device, and click **OK**.

   .. image:: media/create_device.png


After you complete the device registration, you can get the device triple properties: `ProductKey`, `DeviceKey`, and `DeviceSecret`, which will be used in connecting the device to EnOS IoT hub.

## Next Unit

[Configuring Alerts](configuring_alerts)