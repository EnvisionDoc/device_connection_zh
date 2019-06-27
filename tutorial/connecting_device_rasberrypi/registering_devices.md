# Unit 2: Registering the Raspberry Pi on EnOS Console

Before connecting the RPi into EnOS, you need to register the devices on the EnOS Console, including defining models, products, and devices.

## Step 1: Creating Models

In this step, we’ll create 2 models:

**Model 1**: *poc_smartdevice*, which is the model of the device (RPi and sensors as a whole)

**Model 2**: *poc_site*, which is the model of the site where the device is located

Detailed steps are as follows:

1. In the EnOS Console, click **Model** from the left navigation panel.

2. Click **New Model**, provide the following basic settings for the 2 models and click **OK**.

   .. image:: media/model_1.png

   .. image:: media/model_2.png

3. From the model list, click **Edit** for the *poc_site* model, and click **Feature Definition** to define the following attributes:

   .. image:: media/attribute_1.png

4. From the model list, click **Edit** for the *poc_ smartdevice* model, and click **Feature Definition** to define the following measuring points:

   .. csv-table::
      :widths: auto

      "Measuring Point", "Description"
      "*Temperature*", "Double type of data collected from the temperature sensor."
      "*Humidity*", "Double type of data collected from the humidity sensor."
      "*Light*", "Int type of data sending to Raspberry Pi to control the light. Sending 0 stands for light off, sending 1 stands for light on, and sending 2 stands for flickering in this case."
      "*Light_Flicker*", "Int type of data collected from Raspberry Pi to show the light status. Getting 0 stands for light off, and getting 1 stands for light on."

   .. image:: media/attribute_2.png

You can define your own measuring points in your device model by editing your model.



## Step 2: Creating a Product

A product is a collection of devices with the same features. In this tutorial, create an edge product named *smartdevice-NB-IOT* under the *poc_smartdevice* model.

Detailed steps are as follows:

1. In the EnOS Console, click **Device Management** > **Product** from the left navigation panel.

2. Click **New Product**, provide the following basic settings for the product and click **OK**.

   .. image:: media/product.png

## Step 3: Registering Devices

Create an edge device named *smartdevice-NB-IoT*, which belongs to the product created in the previous step.

Detailed steps are as follows:

1. In the EnOS Console, click **Device Management** > **Device** from the left navigation panel.

2. Click **New Device**, provide the following basic settings for the device and click **OK**.

   .. image:: media/device.png

3. Open the **Device Details** page of the *smartdevice-NB-IoT* device, click **Sub-device** > **Add sub device** to add the *smartdevice* as its sub-device (Note: The *smartdevice* is created through the asset tree in the next section).

   .. image:: media/sub_device.png

After you complete the device registration, you can get the device triple properties: `ProductKey`, `DeviceKey`, and `DeviceSecret`, which will be used in connecting the device to EnOS IoT hub.

## Step 4: Binding Devices into Asset Tree

In this tutorial, create an asset tree to manage the topology of the devices.

Detailed steps are as follows:

1. In EnOS Console, click **Asset Tree** from the left navigation panel.

2. Click the + icon to create a root point named *poc_test* using the *poc_site* model.

   .. image:: media/asset_tree_1.png

3. Right-click on the created root node, and select **Add a leaf node > Create asset from model**.

   .. image:: media/asset_tree_2.png

4. Create a device named *smartdevice* under the *poc_smartdevice* model.

   .. image:: media/asset_tree_3.png

## Next Unit

[Configuring Alerts](configuring_alerts)
