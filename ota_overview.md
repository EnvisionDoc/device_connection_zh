# EnOS固件升级
EnOS固件升级通过OTA消息通道，帮助你管理设备固件，以及时修复缺陷、升级功能。

## 主要功能
**设备端SDK**

为设备提供了包含OTA能力的SDK，设备可以上报固件版本信息、请求升级、上报升级结果等。

**管理固件**

管理设备固件的全生命周期。包括固件信息管理，固件的添加、升级、验证、删除等。

**查看固件日志**

查看固件升级全链路日志，追溯设备版本，以便分析升级失败原因。

**统计固件相关数据**

查看统计固件升级相关数据。

## 概念

**OTA固件升级**

通过无线方式（over the air）远程更新设备固件。

**升级策略**

符合升级条件的设备列表的维护方式。

- **快照式升级**

    当前符合升级条件的设备列表为封闭列表，仅升级该列表上的设备。

- **增量式升级**
  
    当前符合升级条件的设备列表为开放列表，后续新增的符合条件设备也将纳入升级范围。

**升级方式**

符合升级条件的设备列表中的设备升级的流程。

- **云端推送升级**


    云端根据升级策略维护待升级设备列表，按照升级序列推送升级请求至设备。设备在线则接受升级请求后开始升级；设备不在线则待设备重连之后接受本次升级请求。

- **设备请求升级**


   云端维护待升级设备列表，待设备主动请求升级后，判断设备是否在可升级范围内。如是，则向设备推送一个可用固件版本。设备确认升级到该版本后即开始升级。
