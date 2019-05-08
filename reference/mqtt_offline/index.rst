基于MQTT协议的设备连接（离线消息）
===========================

你可以根据本章中为你提供EnOS设备协议在第三方系统中配置好相关参数。在第三方系统与EnOS之间建立基于MQTT的数据格式传输，以传输离线消息。

支持的MQTT版本：

- 如果你使用基于证书的双向认证，在端口18883上使用 MQTT v3.1.1。
- 如果你使用基于密钥的单向认证，在端口11883上使用 MQTT v3.1.1。

.. toctree::
   :caption: 通用参数说明
   :maxdepth: 1

   common_param

.. toctree::
   :caption: 建立连接
   :maxdepth: 1

   nonsdk_login

.. toctree::
   :caption: 离线消息集成
   :maxdepth: 1

   report_offline_data_passthrough
   report_offline_events
   report_offline_points
   report_offline_attributes


