# 有关EnOS设备管理

EnOS™设备管理产品帮助你快速将物理设备安全连接至EnOS云端并开始数据传输，管理设备周期，及将物理世界的资产结构映射至数字世界。

## 设备接入<deviceconnectivity>

支持设备和云端之间的双向通信：
- 将数据从设备摄入到云端
- 从云端到设备的远程控制

提供设备端SDK，以支持设备向EnOS IoT Hub传输消息。

- 提供基于MQTT协议的设备SDK，以满足长连接的实时需求。
- 提供基于CoAP协议的设备SDK，以满足短连接的低功率需求。
- 支持面向Java、C和Python编程语言的SDK。

支持各种接入方案并提供相应解决方案，以满足企业异构网络的多种场景需求。有关更多信息，参见[设备接入](learn/connection_scenarios)。

## 设备生命周期管理<devicelifecyclemanagement>

提供完整的设备生命周期管理功能，包括：

- 设备注册
- 设备配置
- 固件升级
- 实时监测
- 设备注销

有关更多信息，参见[设备生命周期管理](learn/device_lifecycle_management)。

## 保护设备与云端的安全<deviceandcloudsecurity>

- 支持_secret-per-device_ authentication机制，有助于降低设备可能遭遇的安全风险。该机制适于以批量方式将预分配的设备密钥烧入到每个芯片中的设备。每个设备在出厂时都载有一个密钥对。

- 支持_secret-per-product_机制，在该机制中，相同产品型号的设备会预烧入密钥对(_product key-secret pair_)。设备能够在认证过程中以动态的方式获取设备密钥。该机制适合批量生产时无法将独特的密钥对烧入每个设备的情况。

- 支持-certificate-based-authenticate机制，在该机制中，通过CA证书机制对数据进行加密和解密，保证设备与云端通信的安全。

有关更多信息，参见[保护设备与云端的安全](learn/deviceconnection_authentication)。

## 相关角色

EnOS设备管理服务主要服务于以下角色：

### 物联网实施人员

物联网实施人员进行现场安装实施，包括安装Edge网关设备，铺设Edge网关到接入设备的通讯线缆，设置设备接入，调试设备与云端之间的通信。

### Edge开发者

Edge开发者负责按照EnOS标准设备协议开发Edge的MQTT客户端应用。此类应用的目标是收集Edge的遥测数据，并通过支持的协议采用支持的格式将数据传输到EnOS云端。

### 资产管理员

资产管理员根据业务案例场景创建资产层次结构（资产树）并管理资产。

### 应用开发者

应用开发者基于EnOS提供的API、SDK获取设备遥测信息和配置信息，以满足特定业务案例场景的需求。

## EnOS设备管理的相关产品及服务

### 数据资产管理

EnOS数据资产管理产品提供实时数据流处理、数据订阅、自定义数据存储策略等服务。[了解更多信息 >>](/docs/data-asset/zh_CN/latest/data_asset_overview)

## 后续操作

学习如何快速在EnOS云端配备典型智能IoT设备并开始在设备和云端之间发送遥测数据:

- [快速入门接入智能设备](quickstart/gettingstarted_device_connection)

学习如何配备通过Edge网关接入的传统工业设备：

- [快速入门接入非智能设备](quickstart/gettingstarted_edge_connection)
