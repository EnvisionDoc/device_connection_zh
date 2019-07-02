# Unit 3: Configuring Alerts

To monitor the state of the RPi device, you can define customized alerts on EnOS Console, including alert severities, alert types, alert content, and alert rules for triggering alerts. Detailed steps are as follows:

## Step 1: Defining Alert Severities

Alerts can be classified by severity levels. In this tutorial, define *Warning* and *Info* severity levels:

1. In the EnOS Console, find the **Alert** service in the left navigation panel.

2. Click **Alert Severity > New Severity** to define a severity level with an ID and description for the severity.

   .. image:: media/alert_severity.png

## Step 2: Defining Alert Types

In this tutorial, define alert types *Humi Status*, *Temp Status*, and *LED Status* to monitor device status.

1. Select **Alert > Alert Type** from the left navigation panel.

2. Click **New Type** and input an ID and description for the severity type.

   .. image:: media/alert_type.png

## Step 3: Defining Alert Content

In this tutorial, define alert content that describes the actual status of the RPi device.

1. Select **Alert > Alert Content** from the left navigation panel.

2. Click **New content** to configure the following alert content.

   - ​Humi High: Humidity is high
   - Temp High: Temperature is high
   - LED Off: LED light is off
   - LED On: LED light is on

   .. image:: media/alert_content.png

## Step 4: Defining Alert Rules

In this tutorial, define alert rules with conditions that trigger the alerts.

1. Select **Alert > Alert Rule** from the left navigation panel.

2. Click **New Rule** to configure the following alert rules:

   - Humi High: Humidity >=70
   - Temp High: Temperature >=30
   - LED Off: Light_Flicker value is 0
   - LED On: Light_Flicker value is 1

   .. image:: media/alert_rule.png

When the alert rule is saved, it will start running to monitor the temperature of the battery device. You can view active alerts and history alerts that reported against the device on the **Alert Record** page.

You can also use the event service APIs to query alert records. For example, using the *Search Active Alerts* API to query active alerts by organization ID and other filtering conditions. For more information about EnOS API, go to **EnOS Console > EnOS API**.

## Next Unit

[Developing Python Program to Enable RPi to Connect](connecting_devices)

<!-- end -->