# Unit 5: Monitoring Device from Cloud

After the device is connected and starts uploading data to EnOS Cloud, you can monitor the device from the cloud.

## Checking Device Status

1. In the EnOS Console, click **Device Management > Device**。
2. Locate the *smartdevice-NB-IoT* and *smartdevice* devices.
3. Check the status of the devices. The status of the devices will change from *inactive* to *online*.

.. image:: media/device_status.png

## Viewing Uploaded Data

The device data uploaded to EnOS Cloud can be viewed on the EnOS Console:  

1. Open the **Device Details** page, click **Measuring Points**.

2. Locate the *Temperature* measuring point, click **View data**.

3. Select a time range and view the device data in a chart or table. See the following example.

   .. image:: media/device_data.png

With storage policy configured, the uploaded device data can be stored for further processing and analysis. For more information, see [Data Asset Management](/docs/data-asset/en/latest/data_asset_overview).

## Viewing Alert Records

1. In the EnOS Console, select **Alert > Alert Record**.
2. Search for active alerts and history alerts reported for the RPi by the asset ID or model ID.
3. Optionally, you can configure alert subscription topics for the alerts, so that any alerts reported will be pushed to your application. For details, see the [Subscribing to Device Real-time Data and Alert Records](/docs/data-asset/en/latest/tutorial/subscribing_to_device_data/index.html) tutorial.

