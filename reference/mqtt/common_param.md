# 通用参数说明

## 请求和回复中的通用参数

以下表格列出了一些通用参数的说明。

.. list-table::
   :widths: auto

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
     - 请求中的参数。可以为int或dict格式。
   * - method
     - String
     - 请求中必需
     - 请求方法。
   * - code
     - Integer
     - 响应中必需
     - 结果返回码，继承云端协议返回码。通用结果返回码，参见`通用返回错误码`_
   * - data
     - JSON
     - 可选
     - 返回的详细信息。根据返回值的不同其可以为数组格式或者字典格式。

## 通用结果返回码

下表列举了所有MQTT topic通用的结果返回码。对于某一个topic独有的返回码，参见有关topic的说明文档。

| 返回码 | 错误信息 | 释义|
|---------|---------|---------|
| 1220 | Payload format error | Payload包含了非法JSON格式 |
| 1251 | Payload is empty | Payload是空的 |
| 1260 | Methods not consistent | 该方法不能用于该topic |
| 1008 | Msg size is too large, discard the message | MQTT消息大小超过限制 |
| 1009 | Publish to topic with no write permission | 不支持该MQTT topic。或者向该topic发布数据的设备尚未登录EnOS |
