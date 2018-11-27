# Edge connection management overview

You can use EnOS™ Edge to ingest data from on-site devices or 3rd party systems to the EnOS Cloud. As the front end of EnOS Cloud, EnOS Edge extends the data ingestion capability through supporting various _communication protocols_ and providing _device templates_ to bridge the heterogeneous actual measuring points and the common device models.

## Concepts

### Device template

A device template is the adapter between a common device model and a specific model of device. A device template allows mapping between a specific device measuring point and a common device model.

Based on industrial experiences, EnOS has accumulated a number of device templates for inquiry and reuse. You can also define custom device templates.

### Communication protocol

Communication protocol is a common phrase used in energy industry. EnOS Edge provides a rich protocol library that contains common protocols of energy and power industry, such as modbus, IEC104, OPCDA, OPCUA, and OPC-XML-DA. In addition, EnOS supports conventions for the major device manufacturers in the industry. For more information, see [Protocols Supported by EnOS edge](https://docs.envisioniot.com/docs/enos-edge/zh_CN/latest/edge_specification/data_ingestion.html).

With these protocols made available, you can browse and select desired protocols in the device template, which can be used after simple configuration, to avoid development or integration of protocols.
