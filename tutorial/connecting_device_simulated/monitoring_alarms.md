#  Unit 5: Monitoring Device Alerts

To monitor the health and performance of your devices, you can define customized alert severities, alert types, alert content, and alert rules against any anomaly of the device. In this tutorial, enable alerts to monitor the temperature of the battery device. Detailed steps are as follows:

1. In the EnOS Console, find the **Alert** service in the left navigation panel.

2. Click **Alert Severity > New Severity** to define a "Warning" severity level.

   .. image:: media/alert_severity.png

3. Click **Alert Type > New Type** to define an "Exceeding_Limit" alert type.

   .. image:: media/alert_type.png

4. Click **Alert Content > New Content** to define the alert content, which can contain the cause of the alert and actions needed from the device owner. Then, select the *battery* model and the defined alert type.  

   .. image:: media/alert_content.png

5. Click **Alert Rule > New Rule** to define the alert rule for monitoring the battery temperature. For example, when the value of the temperature measuring point exceeds 100 degrees, the alert will be triggered.

   .. image:: media/alert_rule.png

When the alert rule is saved, it will start running to monitor the temperature of the battery device. You can view active alerts and history alerts that reported against the device on the **Alert Record** page.

.. image:: media/alert_records.png

You can also use the event service APIs to query alert records. For example, using the `listActiveAlerts` API to query active alerts by organization ID and other filtering conditions. For more information about EnOS API, see [About EnOS APIs](/docs/api/en/latest/overview.html).

## Next Unit

[Monitoring Device State and Data](viewing_data)

<!-- end -->

