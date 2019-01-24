# Edge连接管理

你可以使用EnOS™ Edge将现场设备或第三方系统中的数据提取到EnOS Cloud。作为EnOS Cloud的前端，EnOS Edge通过支持各种_通信协议_和提供_设备模板_来扩展数据提取能力，用以连接异构的实际测量点和通用设备模型。

在edge连接方案中，子设备将数据发送至edge，edge聚集所有子设备的数据并发送至云端。在edge连接中存在两个方向的数据流：
- 由下至上：设备数据的上送
- 由上至下：云端配置的下发

在Edge连接的云端配置中，你需要首先配置云端与edge的连接，然后将子设备关联至连接中。

## 相关概念<edgeconcepts>

### 设备模板<devicetemplate>

设备模板是公共设备模型和特定设备模型之间的适配器。设备模板允许用户在特定设备测量点和公共设备模型之间进行映射。
基于行业经验，EnOS已经积累了许多设备模板，你可对设备模板进行查看或重用。您也可以自定义设备模板。

### 通讯协议<communication>

通信协议是能源工业中的一个常用术语。EnOS Edge提供了丰富的能源和电力行业的通用协议库，如modbus，IEC104，OPCDA，OPCUA和OPC-XML-DA。此外，EnOS还支持业界主要设备制造商。更多信息，参考[数据采集](https://docs.eniot.io/docs/enos-edge/zh_CN/latest/edge_specification/data_ingestion.html).

你可以在设备模板中查看和选择你需要的模板, 并进行相应的改动，用以避免开发或整合这些协议。
