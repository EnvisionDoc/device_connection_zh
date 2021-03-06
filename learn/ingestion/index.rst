﻿数据摄取
========

EnOS™支持以下数据摄取方式：

**场景1: 设备直接接入EnOS Cloud**

在该场景下，EnOS Cloud直接从设备获取实时数据。

**场景2: 设备已接入第三方系统**

在现实中，设备可能已经接入第三方云或系统中，如设备制造商的设备管理系统。

在该场景下，EnOS提供以下数据连接方案：

- 实时路径: 将实时数据从第三方云与系统中转发至EnOS。更多信息，参考 `第三方云数据转发至EnOS Cloud <cloud2cloud_dataforwarding>`__。

- 历史路径: 将第三方系统中的离线历史数据通过消息集成的方式导入EnOS。

不管通过以上哪种方式实现的连接及数据摄取，数据都可以经由IoT Hub被规则引擎路由至流式计算引擎，告警引擎，及根据存储策略进入不同类型的数据库以备其他服务试用。

.. toctree::
   :maxdepth: 1

   data_format
   cloud2cloud_dataforwarding
   offline_message_integration

