# 设备预配

EnOS设备预配功能帮助物联网开发人员进行设备全生命周期管理，该服务建立设备与EnOS Cloud间的数据通道，并保障设备终端与EnOS Cloud间进行安全的双向通信。

## 主要功能<functions>

EnOS接入服务提供以下主要功能：

### 设备接入<deviceconnection>

提供设备端SDK让普通设备或edge接入IoT Hub。
- 提基于MQTT协议的设备SDK，帮助你开发运行在设备或edge上的MQTT Client应用。
- 提供设备直连和网关代理连接等接入方案，为企业异构网络的设备接入的多种场景提供解决方案。

### 设备管理<devicemanagement>

提供完整的设备生命周期管理功能，包括：
- 设备注册
- 设备配置
- 远程控制
- 固件升级
- 实时监测
- 设备注销

### 保护设备与云端的安全<security>

**鉴权**
- 提供基于设备秘钥的设备认证机制，降低设备被攻破的安全风险，适合有能力批量烧入预分配设备密钥到每个芯片的设备。
- 提供基于产品秘钥的设备预烧，认证时动态获取三元组，适合批量生产时无法将三元组烧入每个设备的情况。

更多信息，参考[设备认证机制概述](deviceconnection_authentication)。

**通信和数据安全**
- 通过CA证书机制对数据进行加密和解密，保证设备与云端通信的安全。
- 通过TOPIC对通信资源进行隔离，避免设备访问未经授权的数据。

## 消息流及相关概念<messageflow>

如下图所示, 设备可以直连或通过edge连接至EnOS IoT Hub。EnOS IoT Hub接受直连设备或edge通过MQTT协议进行通信。MQTT是一种轻量级基于TCP/IP的开源物联网协议。EnOS的MQTT协议支持以下功能：
- 数据基于TOPIC的订阅与发布
- RRPC

.. image:: media/device_connection_message_flow.png
   :alt: MQTT-Based Device Connection Message Flow
   :width: 800px

数据通过IoT Hub上送至EnOS Cloud中会由规则引擎分发至不同存储中用于以下用途：
- 时序数据库
- 告警引擎
- 流式计算引擎

设备接入数据流中涉及以下功能模块及概念：

### IoT Hub<iothub>

IoT Hub是EnOS为设备连接提供的云代理服务。IoT Hub有以下功能：
 - 从设备到云端的安全可靠的大规模双向消息传输。
 - 将数据从客户端转发到EnOS上的相应订户。
 - 提供许可授权以及创建事物和策略。
 - 公开连接MQTT客户端的接口。

### Edge网关<edge>

Edge位于Envision EnOS IoT平台的前端，采集现场设备数据或连接到第三方系统，并传输数据到EnOS Cloud。EnOS物联网操作系统支持EnOS Edge和符合EnOS设备数据传输协议的第三方edge产品。

EnOS Edge是一款基于软件的edge产品，支持数据采集，多种通信约定，本地缓存和断点续建。它可以部署在云计算机中，也可以部署在指定品牌型号的现场硬件上。Edge必须含有由Envision分配的合法序列号（SN）才能被EnOS Cloud识别。更多信息，参考[EnOS Edge概述](https://docs.envisioniot.com/docs/enos-edge/zh_CN/latest/edge_overview.html)。

### Topic

Topic是消息的主题。一个topic的信息占用一个连接通道，设备可发布或订阅某个topic的消息。在你进行设备预配时，你需要配置该设备对应的topic名称，当设备连接成功后将向该topic发布或订阅消息以实现与EnOS Cloud的通讯。


## 接入场景<scenarios>

基于edge网关是否以及在何处使用连接，支持以下的连接方案：

.. image:: media/device_connection_methods.png
   :width: 780px

**场景1：设备通过MQTT协议直连至IoT Hub**

此方法要求设备支持MQTT协议，适用于大多数新的IoT设备。

**场景2：设备通过云端EnOS Edge连接**

该场景中，EnOS Edge在云端远程部署。该方法主要适用于不支持MQTT协议的传统设备和系统。

**场景3：设备通过现场EnoS Edge连接**

该场景中，EnOS Edge与设备在现场一起部署。该方法主要适用于不支持MQTT协议的传统设备和系统。

**场景4：设备通过私有协议连接至EnOS Cloud Edge群集**

该场景中，需要连接的设备具有唯一ID，并且可以通过支持的通信协议上载数据。该方法经常用于光伏器件连接。

## 相关信息<relatedinformation>

- [直连设备连接快速入门](gettingstarted_device_connection)
- [子设备通过edge连接至EnOS Cloud快速入门](gettingstarted_edge_connection)
