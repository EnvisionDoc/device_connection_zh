# Unit 3: Configuring Alerts

To monitor the state of the RPi device, you can define customized alerts on EnOS Console, including alert severities, alert types, alert content, and alert rules for triggering alerts. Detailed steps are as follows.

## Step 1: Define Alert Severities

Alerts can be classified by severity levels. In this tutorial, define *High* and *Middle* severity levels:

1. In the EnOS Console, find the **Alert** service in the left navigation panel.

2. Click **Alert Severity > New Severity** to define a severity level with an ID and description for the severity.

   .. image:: media/alert_severity.png

You can define other alert severity levels based on your business model, such as *Error*, *Warning*, *Info*, etc.

## Step 2: Define Alert Types

In this tutorial, define alert types *Temp_Status*, *Humi_Status*, and *LED_Status* to monitor the RPi device status.

1. Select **Alert > Alert Type** from the left navigation panel.

2. Click **New Type** and input an ID and description for the severity type.

   .. image:: media/alert_type.png

## Step 3: Define Alert Content

In this tutorial, define alert content that describes the actual status of the RPi device.

1. Select **Alert > Alert Content** from the left navigation panel.

2. Click **New content** to configure the following alert content.

   - ​Humi_High: Humidity exceeds limit
   - Temp_High: Temperature exceeds limit
   - LED_Off: LED light is off
   - LED_On: LED light is on

   .. image:: media/alert_content.png

## Step 4: Define Alert Rules

In this tutorial, define alert rules with conditions that trigger the alerts.

1. Select **Alert > Alert Rule** from the left navigation panel.

2. Click **New Rule** to configure the following alert rules:

   - Humi_High: Humidity >=70
   - Temp_High: Temperature >=30
   - LED_Off: Light_Flicker value is 0
   - LED_On: Light_Flicker value is 1

   .. image:: media/alert_rule.png

When the alert rules are created, they will start running to monitor the temperature, humidity, and LED light status of the RPi device. You can view active alerts and history alerts that are reported against the device on the **Alert Record** page.

You can also use the event service APIs to query alert records. For example, using the *Search Active Alerts* API to query active alerts by organization ID and other filtering conditions. For more information about EnOS APIs, go to **EnOS Console > EnOS API**.

## Next Unit

[Developing Python Program to Enable RPi to Connect](connecting_devices)

<!-- end -->