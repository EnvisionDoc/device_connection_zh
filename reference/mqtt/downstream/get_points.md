# 获取测点信息

设备收到获取设备测点获取指令之后，通过reply消息向云端返回获取的测点信息。可以通过数据流转获取返回的测点信息。

.. note:: 根据物模型中的输入参数和输出参数来配置下列的参数。

下行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/measurepoint/get`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/measurepoint/get_reply`

## 请求数据格式<request>

```
{
	"id": "123",
	"version": "1.0",
	"params": ["power", "temp"],
	"method": "thing.service.measurepoint.get"
}

```

## 响应数据格式<response>

```
{
	"id": "123",
	"code": 200,
	"data": {
		"power": "on",
		"temp": "23"
	}
}

```

## 参数说明<parameters>

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
     - List
     - 必需
     - 获取测点信息的参数
   * - method
     - String
     - 必需
     - 请求方法
   * - power
     - String
     - 可选
     - 要获取值的测点的标识符，在此示例中为测点 **power** 的标识符。
   * - temp
     - String
     - 可选
     - 要获取值的测点的标识符，在此示例中为测点 **temp** 的标识符。
   * - code
     - Integer
     - 必需
     - 200或设备端定义的错误码，200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式


<!--end-->
