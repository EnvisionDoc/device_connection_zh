# 有关EnOS设备管理

EnOS™设备管理服务帮助你快速将物理设备安全连接至EnOS云端并开始数据传输，管理设备周期，及将物理世界的资产结构映射至数字世界。

## 设备接入<deviceconnectivity>

EnOS设备管理功能帮助物联网开发人员进行设备全生命周期管理，该服务建立设备与EnOS Cloud间的数据通道，并保障设备终端与EnOS Cloud间进行安全的双向通信。

提供设备端SDK让普通设备或edge接入IoT Hub。

- 提基于MQTT协议的设备SDK，帮助你开发运行在设备或edge上的MQTT Client应用。

提供设备直连和网关代理连接等接入方案，为企业异构网络的设备接入的多种场景提供解决方案，参见[设备接入](learn/connection_scenarios)。

## 设备生命周期管理<devicelifecyclemanagement>

提供完整的设备生命周期管理功能，包括：

- 设备注册
- 设备配置
- 固件升级
- 实时监测
- 设备注销

参见[设备生命周期管理](learn/device_lifecycle_management)。

## 保护设备与云端的安全<deviceandcloudsecurity>

### 鉴权

- 提供基于设备秘钥的设备认证机制，降低设备被攻破的安全风险，适合有能力批量烧入预分配设备密钥到每个芯片的设备。
- 提供基于产品秘钥的设备预烧，认证时动态获取三元组，适合批量生产时无法将三元组烧入每个设备的情况。

更多信息，参考[设备认证机制概述](learn/deviceconnection_authentication)。

### 通信和数据安全

- 通过CA证书机制对数据进行加密和解密，保证设备与云端通信的安全。
- 通过TOPIC对通信资源进行隔离，避免设备访问未经授权的数据。

## 相关角色

EnOS设备管理服务主要服务于以下角色：

### 物联网实施人员

物联网实施人员进行现场安装实施，包括安装Edge，铺设Edge到接入设备的通讯线缆；通过设备预配功能进行设备的接入调试，包括创建云端配置和调试通信。

### Edge开发者

Edge开发者可根据EnOS定义的设备端协议进行edge端MQTT Client应用开发，将edge设备收集的数据按照EnOS支持的协议及格式发送至平台。

### 资产管理员

根据业务案例创建资产层次结构（资产树）并管理资产。

### 应用开发者

基于EnOS提供的API、SDK获取设备数据或配置信息，以进行业务应用开发。


## EnOS设备管理的相关服务

### 流式计算服务

该服务提供对组织内资产计算逻辑的定义，订阅资产数据，基于预定义的计算逻辑进行数据计算，例如计算测点的5分钟平均、10分钟平均等。[了解更多 >>](https://www.envisioniot.com/docs/online-data/zh_CN/latest/streaming_overview.html)

## 后续操作

学习如何快速将一个典型的智能设备接入EnOS物联网平台并模拟收发数据:

- [快速入门直连设备至EnOS Cloud](quickstart/gettingstarted_device_connection)

学习如何快速将一个典型的传统工业设备通过edge网关接入EnOS物联网平台并模拟收发数据：

- [快速入门子设备通过edge连接至EnOS Cloud](quickstart/gettingstarted_edge_connection)
