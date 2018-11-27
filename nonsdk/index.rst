基于EnOS设备协议规范的协议开发
===========================

EnOS Cloud为设备端开发提供了SDK，这些SDK已封装了设备端与云端的交互协议,你可以直接使用设备端SDK来进行开发。

如果提供的SDK不能满足你的需求，你可以根据本章中为你提供EnOS 设备协议开发自定义的设备端SDK。在设备与EnOS之间建立基于MQTT的数据格式传输。如果你的数据格式正确，你的设备可随时连接EnOS。


通用参数说明
===========

以下表格列出了一些通用参数的说明。

.. list-table::
  :header-rows: 1

  * - 参数
    - 取值
    - 是否必需
    - 说明
  * - id
    - String
    - 可选
    - 消息ID号，保留值。
  * - version
    - String
    - 必需
    - 协议版本号。
  * - params
    - JSON
    - 请求中必需
    - 请求中的参数。JSON格式。可以为int或dict格式。
  * - method
    - String
    - 请求中必需
    - 请求方法。
  * - code
    - Integer
    - 响应中必需
    - 结果返回码，继承云端协议返回码。
  * - data
    - JSON
    - 可选
    - 返回的详细信息。JSON格式。根据返回值的不同其可以为数组格式或者字典格式。  

支持的协议格式
===================

设备端协议从消息的收发角度可以分为上行请求和下行指令。

详情请参见以下章节。

.. toctree::
  :caption: 上行请求
  :maxdepth: 1

  upstream/device_registration
  upstream/topological_mgmt/index
  upstream/device_connection/index
  upstream/device_tags/index
  upstream/device_else/index



.. toctree::
  :caption: 下行请求
  :maxdepth: 1

  downstream/devices/index
  downstream/subdevices/index
