# 设置测点

.. note:: 根据物模型中的输入参数和输出参数来配置下列的参数。

设备接收到设置设备测点请求信息后会更新设备测点信息。



下行
- TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/measurepoint/set`

- Reply TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/measurepoint/set_reply`

## 请求数据格式

```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"temperature": 30.5
	},
	"method": "thing.service.measurepoint.set"
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
     - 设置设备测点的参数
   * - method
     - String
     - 必需
     - 请求方法
   * - temperature
     - Float
     - 可选
     - 要修改的测点的标识符，在此示例中为测点 **temperature** 的标识符。此处设置的值必须与测点的数据类型匹配。例如，当此参数的数据类型在模型中设置为float时，此处的值必须为float。
   * - code
     - Integer
     - 必需
     - 200或设备端定义的错误码。200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息，JSON格式


<!--end-->
