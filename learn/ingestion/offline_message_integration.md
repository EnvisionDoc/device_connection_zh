# 离线消息集成

用户可以通过离线消息集成功能，将第三方系统中设备的历史数据和离线缓存数据导入EnOS。并设置策略，对集成进来的数据进行处理、存储和计算。

离线消息集成功能仅支持单向的数据传输，即从第三方系统传输到EnOS。可在下列场景中使用离线消息集成功能：

- 第三方系统中设备历史数据导入：如第三方系统过去一个月的存量历史数据；
- 设备离线缓存的数据进行断电续传：如设备离线状态下缓存的测点数据，设备恢复上线后需要重新上报到第三方系统，再传送至EnOS <!--此处的设备，指的是连接到第三方云的设备吗？-->

## 消息源类型

离线消息集成目前支持第三方系统作为客户端，通过下列协议、格式或方式将历史数据和离线缓存数据导入EnOS：

- MQTT。离线消息集成使用的MQTT Topic、请求及响应格式、参数说明，参见[基于MQTT协议的设备连接（离线消息）](../../reference/mqtt_offline/index)
<!--后续如果支持更多协议、格式、方法，请在此处列举。 -->

## 流程概述

基于MQTT协议，将第三方系统的历史数据和离线缓存数据集成至EnOS的流程图如下：

.. image:: ../../media/offline_message_integration_mqtt.png

1. 在EnOS创建模型、产品，关联模型与产品；

2. 预先在EnOS上创建需要导入历史消息的设备；

3. 在EnOS控制台创建MQTT消息通道，获得下列参数：
  - Broker URL
  - ClientID
  - Username
  - Password
  在EnOS控制台选择**设备管理 > 消息集成**，在已创建成功的消息通道处选择**详情**，即可在详情页找到以上参数。

4. 在第三方系统上配置初始化MQTT消息通道的相关参数，即上一个步骤中的MQTT参数；
  
5. 第三方系统发起建立连接的请求；

6. 第三方系统鉴权通过，EnOS成功订阅历史数据集成相关topics；
  
7. EnOS返回一个鉴权成功的响应；

8. 第三方系统将历史消息发布到对应topic，第三方与EnOS使用一个独立的broker发布和订阅数据，而不会复用现有的用于第三方系统实时数据订阅和发布的broker；

9.  （可选）如果第三方发送的设备是透传的原始数据，如二进制数据，则EnOS会调用该设备所属产品的数据解析脚本将其转换为EnOS定义的JSON格式；

10. EnOS会检测第三方上报的历史消息中携带的DeviceKey：
  - 如果用户没有事先将该deviceKey所属的设备注册在EnOS上，EnOS会在消息日志中报错，无法导入相关设备的历史消息;<!--后续如果支持了设备自动注册，这里需要修改。-->
  - 如果deviceKey存在，则保存模型化的历史数据；

11. EnOS向第三方系统发送一个数据集成成功的响应；

12. 用户可在控制台查看导入的指定设备历史消息；

.. note:: 1. 要查看通道的消息日志，选择**设备管理 > 消息集成**，选定消息通道点击**详情**，日志包括下列信息：
  - 通道的连接信息，如MQTT连接、断开记录；
  - Payload传输日志；
  - 错误日志，如JSON格式校验不通过、脚本调用错误等。

有关创建离线消息集成通道的详细操作，参见[创建离线消息集成通道](../../howto/device/manage/creating_offline_message_integration_channel)。

## 离线消息的应用

第三方系统的历史消息集成到EnOS之后，应用程序可以通过EnOS API调用这些数据，参见API相关文档；同时，用户也可以在EnOS上订阅离线消息数据流，离线数据经过规则引擎会被分发到相应的Kafka topic，供EnOS数据资产管理的相关功能调用。

