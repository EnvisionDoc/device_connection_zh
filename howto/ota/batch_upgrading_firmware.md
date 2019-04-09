# 批量升级设备固件

固件如进行过验证操作，即可创建批量升级任务，指定升级策略，升级方式，并开始进行升级消息推送。该文章描述了如何创建一个批量升级任务。

## 任务描述

正在进行固件升级的设备，无法执行新的固件升级任务。

配置批量升级任务需要提供以下关键配置：

### 升级策略

EnOS固件升级OTA服务支持快照式升级和增量式升级。

- **快照式升级**：将当前已上报固件版本且版本号落在升级范围的设备列表作为一个封闭集合，仅升级这个集合中的设备，新添加的设备即使符合升级条件也不会接收到升级推送。

- **增量式升级**：将当前已上报固件版本且版本号落在升级范围的设备列表作为一个开放集合，对于后续新增加的符合条件的设备也会纳入升级范围，待升级设备列表始终处于动态维护的过程中。

### 升级方式

你可以选择云端推送升级或设备请求升级。

- **云端推送升级**：云端根据升级策略维护待升级设备列表，按照升级序列推送升级请求至设备。设备在线则接受升级请求后开始升级；设备不在线则待设备重连之后接受本次升级请求。

- **设备请求升级**：云端维护待升级设备列表，待设备主动请求升级后，判断设备是否在可升级范围内。如是，则向设备推送一个可用固件版本。设备确认升级到该版本后即开始升级。

### 时间窗口

EnOS Cloud推送升级可以选择升级的推送时间段，仅在时间段之内进行升级，该时间段之外不会推送升级请求。EnOS允许的最短时间窗口是30分钟。


## 开始前准备

- 完成[验证固件](verifying_firmware)步骤

## 步骤

1. 选择 **设备管理 > OTA升级** ；

2. 在固件列表选择需要升级的固件，选择 **批量升级** ；

3. 在弹出的批量升级窗口，填入必要参数：

   .. csv-table::
      :widths: auto

      "字段", "含义"
      "待升级版本号", "可选择一个或多个需要升级的版本号。"
      "升级策略", "可选择 **快照式升级** 和 **增量式升级** 。有关升级策略的更多信息，参见[术 语](ota_concept)。"
      "升级范围", "如果需要指定版本号下全部设备，选择 **全部设备** ； 如果需要进一步筛选出一部分设备 进行升级，选择 **定向升级** 。"
      "筛选方式", ""
      "", "当升级范围为 **定向升级** 时可用，可按照设备DeviceKey、标签值、属性值、所在资产树筛选出待升级设备。"
      "设备的DeviceKey", "筛选方式为 **指定设备DeviceKey** 时可见，可以输入版本号以筛选设备。"
      "设备标签", "筛选方式为 **指定设备标签值** 时可见，可以根据标签和标签值筛选设备。"
      "设备属性", "筛选方式为 **指定设备属性值** 时可见，可以根据属性名和属性值筛选设备。"
      "资产树", "筛选方式为 **指定资产树** 时可见，可以根据OU内的根结点以及根结点下的父节点选择设备，根结点或父节点本身如果也是设备，也会纳入升级范围；如果要指定底层子节点的设备，应通过设备DeviceKey来进行。最多可以选择5个资产树。"
      "升级方式", "包括云端推送升级和设备请求升级。如果选择通过指定设备DeviceKey、标签、属性和资产树筛选设备定向升级，则升级方式只能为“云端推送升级”。"
      "时间窗口", "默认时间段为24小时。EnOS Cloud仅在时间窗口之内推送升级。"

4. 选择 **确定** ， 开始批量升级流程；

## 结果

在 **OTA升级** 中，在固件上选择 **升级详情** ，进入升级详情页，点击 **升级成功** **升级失败** 标签页，可查看批量升级的结果，参考[查看升级详情](viewing_upgrade_detail)

## 后续操作

升级完成后，你可以删除不再需要的固件，参考[删除固件](deleting_firmware)。