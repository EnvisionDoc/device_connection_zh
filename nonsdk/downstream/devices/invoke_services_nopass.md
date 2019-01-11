# 调用设备服务 (非透传)

设备端通过服务调用TOPIC响应设备服务调用请求，云端采用同步或异步的方式推送该消息，设备端可以利用reply topic响应该消息。

下行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}_reply`

.. note:: `tsl.service.identifier` 为TSL模板中事件的描述符。


## 请求数据格式

```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"Power": "on",
		"WindState": 2
	},
	"method": "thing.service.{tsl.service.identifier}"
}
```

## 响应数据格式

```
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
     - List
     - 必需
     - 调用设备服务的参数
   * - method
     - String
     - 必需
     - 请求方法
   * - Power
     - String
     - 可选
     - 要调用的服务的标识符，在此示例中为服务 **Power** 的标识符。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为string时，此处的值必须为string。
   * - WindState
     - integer
     - 可选
     - 要调用的服务的标识符，在此示例中为服务 **Windstate** 的标识符。如上，此处设置的值必须与服务的数据类型匹配。
   * - code
     - Integer
     - 必需
     - 200或设备端定义的错误码。200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式


<!--end-->
