# 设备接入概述

EnOS接入服务帮助物联网开发人员进行设备全生命周期管理，该服务建立设备与EnOS Cloud间的数据通道，并保障设备终端与EnOS Cloud间进行安全的双向通信。

## 概念<concepts>

**模型**

模型是对接入物联网的对象的功能的抽象。模型定义了对象的属性，测点，服务，和事件四个维度的功能。更多信息，参考[模型概述](model_overview)。一个模型可被关联至多个_产品_。

**产品**

产品，或称为产品型号，是一组具有相同功能的设备的集合。在模型的基础上，产品进一步定义了设备接入物联网的通信方式。一个产品可包含多个_设备_。

**设备**

设备是模型的实例。一个设备属于某一种产品。


## 主要功能<functions>

EnOS接入服务提供以下主要功能：

## 设备接入<deviceconnection>
提供设备端SDK让设备边界接入IoT Hub。
- 提供MQTT设备SDK，既满足长连接的实时性需求，也满足短连接的低功耗需求。
- 提供设备直连和网关代理连接等接入方案，为企业异构网络设备接入的多种场景提供解决方案。

## 设备管理<devicemanagement>

提供完整的设备生命周期管理功能，包括：
- 设备注册
- 设备配置
- 远程控制
- 固件升级
- 实时监测
- 设备注销

## 模型管理<modelmanagement>
提供设备模型。设备模型使得来自众多厂家、成千上万种型号的设备能够统一于少量的通用模型，从而为简化基于设备数据的应用开发。

## 设备与云端的安全机制<security>

**鉴权**
- 提供基于设备秘钥的设备认证机制，降低设备被攻破的安全风险，适合有能力批量烧入预分配设备密钥到每个芯片的设备。
- 提供基于产品秘钥的设备预烧，认证时动态获取三元组，适合批量生产时无法将三元组烧入每个设备的情况。

更多信息，参考[设备认证机制概述](deviceconnection_authentication)。

**通信和数据安全**
- 通过CA证书机制对数据进行加密和解密，保证设备与云端通信的安全。
- 通过topic对通信资源进行隔离，避免设备访问未经授权的数据



## 技术架构<architecture>

如上图所示, 设备可以直连或通过edge连接至EnOS IoT Hub。EnOS IoT Hub接受直连设备或edge通过MQTT协议进行通信。MQTT是一种轻量级基于TCP/IP的开源物联网协议。EnOS的MQTT协议支持以下功能：
- 数据基于Topic的订阅与发布
- RRPC

![Device Connection Architecture](media/device_connection_methods.png)


基于edge设备是否以及在何处使用连接，支持以下的连接方案：


**场景1：通过MQTT协议（云服务）与EnOS IoT Hub连接**

此方法要求设备支持MQTT协议，适用于大多数新的IoT设备。


**场景2：现场直接EnOS Edge连接**

该场景中，EnOS Edge与设备在现场一起部署。该方法主要适用于不支持MQTT协议的传统设备和系统。

**场景3：通过远程EnOS Edge连接**

该场景中，EnOS Edge在云端远程部署。该方法主要适用于不支持MQTT协议的传统设备和系统。


**场景4：通过EnOS Cloud Edge群集连接（云服务）**

该场景需要连接的设备具有唯一ID，并且可以通过支持的通信协议上载数据。该方法经常用于光伏器件连接。



数据通过IoT Hub上送至EnOS Cloud中会由规则引擎分发至不同存储中用于以下用途：
- 时序数据库 （time series database）
- 告警引擎（event engine）
- 流式计算引擎 （stream computing engine）

## 功能模块<modules>

### IoT Hub<iothub>

IoT Hub是EnOS为设备连接提供的云代理服务。

云代理有功能：

 - 从设备到云端的安全可靠的大规模双向消息传输。

 - 将数据从客户端转发到EnOS上的相应订户。

 - 提供许可授权以及创建事物和策略。

 - 公开MQTT客户端连接的接口。

### EnOS™ Edge<enosedge>


Edge是用于数据采集的Envision EnOS IoT平台的前端。它用于收集现场设备数据或连接到第三方系统，以便将数据采集和传输到EnOS Cloud。 Edge作为软件，支持数据采集，多种通信约定，本地缓存和断点续建。它可以部署在云计算机中，也可以部署在指定品牌型号的现场硬件上。Edge必须含有由Envision分配的合法序列号（SN）才能被EnOS Cloud识别。更多信息，请参考[EnOS™ Edge 概述](https://docs.envisioniot.com/docs/enos-edge/en/latest/edge_overview.html)。
