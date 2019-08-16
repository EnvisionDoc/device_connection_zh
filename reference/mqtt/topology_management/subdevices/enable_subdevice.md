# 子设备启用

用于通知网关下的子设备启用的通知消息，云端使用异步方式发送该消息到网关的TOPIC，通知网关其下属的具体子设备被启用。

下行
- 请求TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/enable`

- 响应TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/enable_reply`

.. note:: TOPIC中的 *productKey* 和 *deviceKey* 为网关的三元组。

## 请求数据格式

```
"id": "123",
"version": "1.0",
"params": [
          {
  "productKey": "xxx",
  "deviceKey": "xxx"
}
]，
"method": "combine.enable"
}

```

## Example of the response message

```
{
	"id": "123",
	"code": 200,
	"data": {}
}

```

## 参数说明​

.. list-table::
   :widths: auto

   * - 参数
     - 类型​
     - 是否必需
     - 描述
   * - Id
     - Long
     - 可选
     - 消息ID号，保留值
   * - version
     - String
     - 必需
     - 协议版本号，目前协议版本1.0
   * - params
     - Object
     - 必需
     - 请求参数
   * - productKey
     - String
     - 必需
     - 子设备的productKey
   * - deviceKey
     - String
     - 必需
     - 子设备的deviceKey
   * - method
     - String
     - 必需
     - 请求方法
   * - code
     - Integer
     - 必需
     - 200或设备端定义的错误码。200代表请求成功执行
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式

<!--end-->
