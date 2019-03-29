# 设备事件上报

如果使用非透传模式，设备需要按照EnOS Cloud中定义的标准格式(当前为JSON)生成数据，然后上报数据。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post_reply`

## 请求数据格式

```
{
  "id": "123",
  "version": "1.0",
 "params": {
		"events": {
			"Power": {
				"value": 1.0,
				"quality": 9
			},
			"temp": 1.02,
			"branchCurr": [
				"1.02", "2.02", "7.93"
			]
		},
		"time": 123456
	},
  "method": "thing.event.{tsl.event.identifier}.post"
}

```

## 响应数据格式

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

## 参数说明

.. list-table::
   :widths: auto

   * - 参数
     - 类型
     - 是否必需
     - 描述
   * - id
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
     - 设备事件上报的参数
   * - events
     - Object
     - 必需
     - 需上报的事件的标识符的列表。
   * - power
     -  --
     - 可选
     - 要上报的事件的标识符。在本例中，为参数 **power** 。此处设置的格式必须与服务的数据类型匹配。例如,当此参数的数据类型在模型中设置为struct时，此处的格式必须与模型中保持一致，为 **value** 和 **value2** 。
   * - value
     - Integer
     - 可选
     - 该事件的标识符。在本例中，为参数 **value** 。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为integer时，那么值必须为integer。
   * - quality
     - Integer
     - 可选
     - 该事件出参的名称。在本例中，为参数 **quality** 。如上，此处设置的格式必须与服务的数据类型匹配。
   * - temp
     - Integer
     - 可选
     - 要上报的事件的标识符。在本例中，为参数 **temp** 。如上，此处设置的格式必须与服务的数据类型匹配。
   * - branchCurr
     - Array
     - 可选
     - 要上报的事件的标识符。在本例中，为参数 **branchCurr** 。如上，此处设置的格式必须与服务的数据类型匹配。
   * - time
     - String
     - 可选
     - 测点的时间戳。如果为空，时间戳会被设为服务器的时间。
   * - method
     - String
     - 可选
     - 请求方法
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行。
   * - data
     - String
     - 可选
     - 返回的详细信息 。JSON格式



<!--end-->
