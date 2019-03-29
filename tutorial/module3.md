# Module 3. Configuring EnOS™ cloud

EnOS™ cloud configuration mainly consists of the following steps:

.. image:: media/module3_main_steps.png

## Step 1: Accessing the EnOS™ Console from EnOS Developer Center

If an EnOS™ Developer Center account is already available, ask the trainer to add the account to the **Device Access Training** user group under `EnOS_Training_CN` organization (Origination Unit).

If an EnOS™ Developer Center account is not available, apply for an account first and then ask the trainer to add the account to the corresponding user group in the above organization.

After the application request is approved, log into the EnOS™ Developer Center through <https://developer.envisioncn.com>, and click **Console**.

## Step 2: Create a new site and a new device

1. Under the **Asset Management > Site and Devices** menu, click **New Site**.

   .. image:: media/module3_newsite.png

2. Fill the site attribute information. Click **Add Domain**, select **Site Type**, and fill the site information, as shown in the figure below:

   .. image:: media/module3_site_type.png

3. Click **Add Device**, and select the device model to create an instance of model. Fill the device information, and save the configuration as shown in the following figure:

   .. image:: media/module3_devicemodel.png

## Step 3: Attach the site to the corresponding organization node

Under the **Asset Management > Asset Tree** menu, attach the site created during the above step to the organization node under **Device Connect Training** organization node, as shown in the figure:

.. image:: media/module3_assettreedevicetraining.png

## Step 4: Create a Device Template

1. Under the **Asset Management > Templates** menu, click **New  Template**, as shown in the figure below:

   .. image:: media/module3_device_template.png

2. Fill and save the basic template information, as shown in the figure below.

   - **Domain**: EnOS_Lab_Domain
   - **Device Type**: Meter

   .. image:: media/module3_create_device_template.png

   .. image:: media/module3_device_template_meter.png


3. Click |edit| edit the new device template;

4. Select the protocol for the template. In this lab, the edge uses ModbusRTU protocol to communicate with EnOS cloud, and Edge acts as the TCP Client and the simulated device acts as the TCP Server.

   - **Protocol Type**: ModbusRTU
   - **Protocol**: ModbusRTU-Client-Std V1.1_Release

   .. image:: media/module3_potrol_type.png

5. Click **Template** next to `config.sys` to download the template.

   .. image:: media/module3_config_sys.png

   Modify the downloaded template and upload the configuration, as shown in the figure below:

   .. image:: media/module3_note_config_sys.png

   You can use the default configuration of `Config.sys` and uploaded directly. Further modifications can be made if necessary.

   After successfully uploaded, `config.sys` turns green.

6. Click **Template** next to `point.csv` to download the data point template. Modify and save the configuration, as shown in the figure below:

   .. image:: media/module3_download_point_csv.png

   .. note:: Please complete the configuration according to the above figure, and save it as a CSV file after configuration. Before uploading this `point.csv` file, use NotePad++ to convert the `point.csv` file into the UTF-8 code format.

   .. image:: media/module3_transfer_utf8.png

   After successful uploaded, `point.csv` will be shown in green, and the point table contents of point.csv will be shown in the lower preview field.

7. Configure the mapping between the aquisition point and model point, as shown in the figure below.

   - Domain point: Meter.Voltage <-> Aquisition point: ai.1
   - Domain point: Meter.LightOn <-> Aquisition point: ao.6

   .. image:: media/module3_edit_mapping_point.png

   After completing the mapping, you will see the results in the list.

   .. image:: media/module3_mappingfinish.png

8. Click **Save** to save the device template configuration.

   .. image:: media/module3_confirmmapping.png

## Step 5: Activate Edge and complete relevant connection configuration

1. In **Asset Management** > **Edge Connections** menu, locate and open the site you created.

2. Click **Activate Edge** from the screen, and enter the Edge name, serial number and MAC address, as shown in the figure below:

   .. image:: media/module3_activate_edge.png

   - **Box Name** : named the edge as you wish.
   - **Serial number** : provided by the trainer in advance.
   - **MAC Address**(*Not necessary* ): input your computer's mac addres.

   .. note:: The Edge serial number is provided by the trainer in advance, and the system will verify the validity of the serial number. After the validation succeeds, you can proceed to associate device template.

3. Add the connection. Select TCP/IP client as the connection method. IP should be the IPv4 address of the local PC during filling of test information. The port number should be 502 (depending on configuration in the modbus slave protocol simulator, Port 502 in this test), as shown in the figure below:

   .. image:: media/module3_addconnection.png

   .. image:: media/module3_addconnection2.png

   - **Name** : named the edge as you wish.
   - **Basic Info** : TCP/IP Client
   - **Address(Primary)**: input your computer's IP addres and port 502.

4. Add the device and associate the new device template, as shown in the figure below:

   .. image:: media/module3_add_device.png

   When you have multiple devices in the site, all devices in this site are shown in the device list here. Select the device, and then select a device template to associate from the device template field below.

   .. note:: If multiple devices are selected at the same time, they have to be of the same device model.

   .. image:: media/module3_add_device_to_connection.png

   Select the device and associate the device template. After saving, the device will be added to the connection.

5. Click **Publish** to publish the configuration to Edge.

   .. note:: Configurations are packed into a configuration file. After you click **Publish**, the file is downloaded to the corresponding Edge.

   .. note:: The current publication status will be displayed on the right. Typically the publication process lasts for 2 minutes. A successful publication will be indicated as **Success**, and the time of the latest successful publication will be displayed.

## Step 6: Test and debug the connection

After you have successfully published the above configuration, you can view the connection status and check the data aquired from your computer through the following procedure:

1. Check the connection status indicator.

   - If the connection is abnormal, the indicator is red.
   - If the connection is normal, the indicator is green.

   The following screenshot shows that the configuration is successfully published. However, the connection status is abnormal.

   .. image:: media/module3_add_device_success3.png

2. Debug the connection. Click **Test** to further analyze the connection issue as shown in the figure below:

   .. image:: media/module3_test1.png

3. Click **View** to view the data collected.

   .. note:: You can view the data only after the **Start** button is clicked.

   .. image:: media/module3_test_start.png

   - Click the **Data** tab to see whether the data have been received, and check consistency between the data received here and the data source (note that the data here are original data without calculation).

     If the data are not correct, you might need to adjust the settings in the `config.sys` and `point.csv` configuration files in the device template screen (for detailed settings in config.sys and point.csv in modbus protocol, see the annex).

   - Click the **Datagram** tab to check if he message has been received, and analyze if the message is reasonable.

   - Click the **Log** tab to analyze the log.

   - Click the **Console** tab to perform test actions such as ping, local IP inquiry, telnet, and viewing TCPconnection  (netstat). These actions are intended to help analyze problems.

     In this test: **Telnet** command can be used to check if Edge can get access to Port 502 of the local PC normally, as shown in the figure below:

   .. image:: media/module3_test_eg.png


## Step 7: Preview the data

The original device data values aquired through Edge can be viewed in the connection test. The data aquisition points are mapped to the model points (ca  be simple one-to-one mapping, or complex mapping after calculation with certain formulas).

You can view the real-time and historical data mapped to the model through the **Asset Management > Data Preview** menu.

.. note:: If the model point is based on calculation from original data of the aquisition point, the data shown here is the value after calculation.*

1. View the real-time data.

   Locate the asset node (can be a site or a device) in the asset tree list as shown in the following figure:

   .. image:: media/module3_data_preview.png

   The real-time data of the acquisition points can be viewed in the right panel.

   .. image:: media/module3_data_preview_comm_traffic.png

   .. note:: This test uses Modbus Slave simulator as the data source. Consistency between data source and final data can be checked (ratio difference may exist between data depending on coefficient setting in the `point.csv` file).

2. View the historical data.

   Click the |search| icon of the data record to view historical data of the current model point.

   - You can view the graphical trend that shows the point value dynamics in the **Trendmap** tab.

   - You can also view the history data as datasheets through the **Datasheet** tab. In this tab, you can export the history data.

   .. image:: media/module3_data_preview_check.png

   .. image:: media/module3_data_preview_check_comm_traffic.png


.. |edit| image:: media/module3_edit_new_device_template.png

.. |search| image:: media/module3_search_button.png


<!--end-->
