# 验证固件

新增固件之后，在批量升级设备固件之前，应当选择一个或多个待升级版本，指定小批设备进行固件升级，验证固件能否正常推送到设备端以及固件能否正常升级，避免大规模升级错误固件导致损失。

.. note:: EnOS仅提供验证升级的消息通道，开发者需要自行验证固件是否可正常推送到设备以及设备更新固件后是否能正常运行。

## 任务描述

本文介绍了批量验证设备固件的步骤。设备固件未经验证的，不能批量升级。已经处于固件升级状态的固件，无法执行新的升级任务。

## 开始前准备

- 已经完成了[新增固件](adding_firmware)。
- 设备已经上报了固件版本和`deviceKey`。

## 步骤

1. 选择 **设备管理 > OTA升级**；

2. 找到需要验证的设备，选择 **验证固件**，选择待升级版本号和符合版本号要求的设备的deviceKey；如果之前进行过验证操作，此时会弹出最近一次验证的结果，点击 **重新验证** 即可弹出输入待升级版本号和设备deviceKey的窗口；

   .. csv-table::
      :widths: auto

      "字段", "注释"
      "待升级版本号", "由设备上报的版本号构成的列表，如设备未上传任何版本信息，则显示 **设备未上报版本号**"
      "设备的DeviceKey", "符合用户选定的版本号的设备deviceKey，每次验证固件，最多可选5个设备"

3. 点击 **验证**，开始设备固件验证，设备固件升级状态表的各项含义如下；

   .. csv-table::
      :widths: auto

      "字段", "含义"
      "deviceKey", "进行固件验证的设备deviceKey"
      "升级状态", "尚未收到云端升级推送的设备，状态为 **待升级**；已经上报了开始升级的设备，状态为 **升级中**；已经完成升级的设备，状态为 **升级成功** 或 **升级失败**"
      "取消升级", "对于状态为 **待升级** 的设备，可以点击 **取消升级** 取消固件升级任务。对于其他状态的设备，**取消升级** 被禁用。"

## 结果

进行固件验证的设备可能出现 **升级成功** 或者 **升级失败** 两种结果。无论结果如何，固件的状态都会变成**已验证**。可以选择 **重新验证** ，选择待升级版本号和deviceKey后再次发起验证；或选择 **知道了** 结束验证流程。

.. note:: 如果有设备处于 **待升级** 状态，选择 **重新验证** 将取消该设备即将进行的固件升级任务；在弹出的固件验证窗口中，可以再次选择同样的固件版本，重新选择刚才被取消任务的设备和**升级失败**的设备重新验证固件。

.. note:: 设备固件升级之后，其功能是否可用，需由开发者根据设备情况自行验证。

## 后续操作

[批量升级](batch_upgrading_firmware)
