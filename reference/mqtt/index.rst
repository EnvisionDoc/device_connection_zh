基于EnOS设备协议规范的协议开发
===========================

EnOS Cloud为设备端开发提供了SDK，这些SDK已封装了设备端与云端的交互协议，你可以直接使用设备端SDK来进行开发。

如果提供的SDK不能满足你的需求，你可以根据本章中为你提供EnOS设备协议开发自定义的设备端SDK。在设备与EnOS之间建立基于MQTT的数据格式传输。如果你的数据格式正确，你的设备可随时连接EnOS。

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
   :caption: Upstream
   :maxdepth: 1

   upstream/device_registration
   upstream/topological_mgmt/index
   upstream/device_connection/index
   upstream/device_tags/index
   upstream/device_else/index


.. toctree::
   :caption: Downstream
   :maxdepth: 1

   downstream/devices/index
   downstream/subdevices/index
