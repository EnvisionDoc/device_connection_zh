数据摄取
========

EnOS supports the following data ingestion scenarios:

**Scenario 1: Devices are connected to the EnOS Cloud**

In this scenario, EnOS Cloud ingests real-time data directly transmitted from devices.

**Scenario 2: Devices are connected to a 3rd-party cloud**

In reality, devices might already be connected to a 3rd-party cloud or system, for example, to the system of the device manufacturer.

In this scenario, EnOS provides the following solutions:

- Real-time data path: Forward the real-time data from the 3rd-party cloud to EnOS cloud.
  For more information, see `Cloud-to-cloud Data Forwarding <cloud2cloud_dataforwarding>`__.

- Historical data path: Integrating the historical data from the 3rd-party system to EnOS through the same channel as the real-time data, that is to ingest historical data into IoT Hub through MQTT protocol.

.. toctree::
   :maxdepth: 1

   data_format
   cloud2cloud_dataforwarding
