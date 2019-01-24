# 有关EnOS设备管理

EnOS设备管理服务帮助你快速将物理设备安全连接至EnOS云端并开始数据传输，管理设备周期，及将物理世界的资产结构映射至数字世界。

## 相关角色

EnOS设备管理服务主要服务于以下角色：

**物联网实施人员**

物联网实施人员进行现场安装实施，包括安装Edge，铺设Edge到接入设备的通讯线缆；通过设备预配功能进行设备的接入调试，包括创建云端配置和调试通信。

**Edge开发者**

Edge开发者可根据EnOS定义的设备端协议进行edge端MQTT Client应用开发，将edge设备收集的数据按照EnOS支持的协议及格式发送至平台。

**资产管理员**

根据业务案例创建资产层次结构（资产树）并管理资产。

**应用开发者**

基于EnOS提供的API、SDK获取设备数据或配置信息，以进行业务应用开发。


## 主要功能

**设备建模**

设备模型使得成千上万来自不同制造商的设备统一于少量的通用模型，从而简化应用的处理。基于行业经验，EnOS模型库积累了大量可复用的模型。[了解更多 >>](model/model_overview)

**设备预配**

帮助实现设备全生命周期管理，建立设备与EnOS Cloud间的数据通道，并保障设备终端与EnOS Cloud间进行安全的双向通信。[了解更多 >>](deviceconnection_overview)

**资产管理**

帮助资产管理员（他们了解企业资产管理的业务）基于物理世界的资产结构快速在云端创建和管理资产的拓扑结构。[了解更多 >>](asset_tree/assettree_overview)

## EnOS设备管理的相关服务

**流式计算服务**

该服务提供对组织内资产计算逻辑的定义，订阅资产数据，基于预定义的计算逻辑进行数据计算，例如计算测点的5分钟平均、10分钟平均等。[了解更多 >>](https://docs.eniot.io/docs/online-data/zh_CN/latest/streaming_overview.html)

**告警服务**

该服务提供对组织内资产异常状态的告警定义，接收资产告警上报和处理，以及针对资产数据定义告警触发触发条件，满足对资产的实时状态监控和故障分析等业务需求。[了解更多 >>](https://docs.eniot.io/docs/event-management/zh_CN/latest/alert_overview.html)

## 后续操作

学习如何快速将一个典型的智能设备接入EnOS物联网平台并模拟收发数据:

- [快速入门直连设备至EnOS Cloud](gettingstarted_device_connection)

学习如何快速将一个典型的传统工业设备通过edge网关接入EnOS物联网平台并模拟收发数据：

- [快速入门子设备通过edge连接至EnOS Cloud](gettingstarted_edge_connection)
