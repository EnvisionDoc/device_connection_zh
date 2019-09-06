# 基于MQTT协议的连接

如下图所示, 设备可以直连或通过edge连接至EnOS™ IoT Hub。EnOS IoT Hub接受直连设备或edge通过MQTT协议进行通信。

- MQTT是一种轻量级基于TCP/IP的开源物联网协议。EnOS的MQTT协议支持以下功能：
  - 数据基于TOPIC的订阅与发布；
  - RRPC；

.. image:: ../media/device_connection_methods.png


数据通过IoT Hub上送至EnOS Cloud中会由规则引擎分发至不同组件中用于以下用途：

- 实时数据库redis，时序数据库，及归档数据库。
  - redis数据库存放所有最新时间点的设备数据；
  - 时序数据库根据存储策略的定义存储访问频次较高的用户指定测点的数据，存储时间为用户指定时长，如1个月，6个月，1年，的设备数据。EnOS默认只通过redis存储最新的设备数据，你必须配置存储策略才能将一段时间内的数据存至时序数据库，并通过时序数据库对应的API访问数据。[了解更多信息>>](/docs/data-asset/zh_CN/latest/learn/storage_policy_overview)
  - 归档数据库根据存储策略的定义访问访问频次较低的历史相对更加久远的数据；

- 告警引擎，根据配置的触发条件生成告警记录。

- 流式计算引擎，根据配置的流式计算处理逻辑对实时数据进行处理并返回给数据中心以供其他服务模块使用或API获取。EnOS提供了简单易用的图形界面的流式分析IDE以帮助提升流数据分析处理的效率。[了解更多信息>>](/docs/data-asset/zh_CN/latest/learn/index)

设备接入数据流中涉及以下功能模块及概念：

## IoT Hub<iothub>

IoT Hub是EnOS为设备连接提供的云代理服务。IoT Hub有以下功能：
- 从设备到云端的安全可靠的大规模双向消息传输。
- 公开连接MQTT客户端的接口。
- 将数据从客户端转发到EnOS上的相应订户。
- 提供许可授权以及创建事物和策略。

## Edge网关<edge>

Edge位于Envision EnOS IoT平台的前端，采集现场设备数据或连接到第三方系统，并传输数据到EnOS Cloud。EnOS物联网操作系统支持EnOS Edge和符合EnOS设备数据传输协议的第三方edge产品。

EnOS Edge是一款基于软件的edge产品，支持数据采集，多种通信约定，本地缓存和断点续建。它可以部署在云计算机中，也可以部署在指定品牌型号的现场硬件上。Edge必须含有由Envision分配的合法序列号（SN）才能被EnOS Cloud识别。更多信息，参考[EnOS Edge概述](/docs/enos-edge/zh_CN/latest/edge_overview.html)。

## 接入场景<scenarios>

基于edge网关是否以及在何处使用连接，支持以下的连接方案：

### 场景1：设备通过MQTT协议直连至IoT Hub

此方法要求设备支持MQTT协议，适用于大多数智能IoT设备。

### 场景2：设备通过云端EnOS Edge连接

该场景中，EnOS Edge在云端远程部署。该方法主要适用于不支持MQTT协议的传统设备和系统。

### 场景3：设备通过现场EnoS Edge连接

该场景中，EnOS Edge与设备在现场一起部署。该方法主要适用于不支持MQTT协议的传统设备和系统。对比场景2的云端Edge部署方式，现场Edge部署方式可帮助实现与云端网络连接断开情况下的端点续传。

### 场景4：设备通过私有协议连接至EnOS Cloud Edge群集

该场景中，需要连接的设备具有唯一ID，并且可以通过支持的通信协议上载数据。该方法经常用于光伏器件连接。
