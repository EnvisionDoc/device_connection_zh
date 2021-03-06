# 设备生命周期管理

EnOS™设备管理产品可帮助你管理从计划和设计阶段到设备退役阶段的端到端设备生命周期。

下图展示了EnOS设备管理产品在设备生命周期各阶段提供的服务：

.. image:: ../media/device_lifecycle_management.png

## 计划和设计阶段

在计划和设计阶段，你将确定如何提取设备模型、管理资产层级。你还将基于待接入的设备类型、设备部署条件和安全需求确定拟使用的接入方案。

<!--我们将在此处链接到计划和设计阶段最佳实践讨论相关话题-->

### 设备建模

应用开发者负责将业务需求转化为数据和处理逻辑需求，可以使用设备模型来描述来自不同制造商的数千个模型，将其统一到较少数量的通用模型之中，进而推动应用处理。EnOS™ Cloud的模型库中积累了一个大型的设备模型集，以便于提取资产数据。[了解更多 >>](../howto/model/model_overview)。


### 资产树管理

资产管理员（他们了解企业资产管理业务）能够快速在云端创建资产的拓扑结构，以对资产进行管理。[了解更多 >>](../howto/asset_tree/assettree_overview)。

### 计划接入方式

通常根据设备的硬件功能及对设备接入的安全性要求选择接入方案。

有关接入场景的更多信息，参见[设备接入](connection_scenarios)。

有关安全选项和机制的更多信息，参见[保护设备与云端的安全](deviceconnection_authentication)。

## 预配阶段

在预配阶段，物联网实施人员会基于所选的接入方式：
- 注册设备，以获得设备身份，并在需要对证书进行认证时，生成证书。
- 使用设备SDK执行设备端开发，以便设备认证到云端并开始传输数据。

学习如何快速在EnOS云端配备典型智能IoT设备并开始在设备和云端之间发送遥测数据:

- [快速入门接入智能设备](../quickstart/gettingstarted_device_connection)

学习如何配备通过Edge网关接入的传统工业设备：

- [快速入门接入非智能设备](../quickstart/gettingstarted_edge_connection)

了解如何采用基于x.509证书的认证：

- [通过基于证书的认证确保接入安全性](../quickstart/gettingstarted_java_ssl_connection)

## 服务阶段

在服务阶段，你可以：

- 通过EnOS在控制面板中原生提供的设备管理仪表板获得设备与消息动态的概览信息。你还可以开发你自己的数据呈现应用。
- 从云端对设备进行远程控制，以启用或禁用设备，或触发设备模型所定义的服务。
- 使用资产警报服务监控设备上的警报（由警报触发规则针对实时测量点遥测数据而定义）。有关更多信息，参见[设备警报](../howto/alert/alert_overview)。
- 根据业务需求管理、使用和存储设备数据。有关更多信息，参见[数据资产管理](/docs/data-asset/zh_CN/2.0.9/data_asset_overview.html)。

## 维护阶段

在维护阶段，你可以：

- 使用OTA服务管理云端的设备固件并通过空中传输的方式升级固件。有关更多信息，参见[以空中传输方式升级固件](../howto/ota/ota_overview)。

- 在证书到期时更新设备证书。

## 退役阶段

在退役阶段，你可以：
- 禁用并从云端删除设备的数据孪生。
- 撤销发布到设备的设备证书。
- 归档已停用设备的数据。