# 上报单个离线测点信息

.. note:: 根据物模型中的输入参数和输出参数来配置下列的参数。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/measurepoint/resume`
- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/measurepoint/resume_reply`

## 请求数据格式

``` json
{
    "id":"123",
    "version":"1.0",
    "params":{
        "measurepoints":{
            "Power":{
                "value":1,
                "quality":9
            },
            "temp":1.02,
            "branchCurr":[
                "1.02",
                "2.02",
                "7.93"
            ]
        },
        "time":123456
    },
    "method":"thing.measurepoint.resume"
}
```

## 响应数据格式

``` json
{
    "id":"123",
    "code":200,
    "data":{}
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
     - 上报测点所需的参数
   * - method
     - String
     - 必需
     - 请求方法
   * - measurepoints
     - Object
     - 必需
     - 需上报的测点的标识符的列表。
   * - power
     - String
     - 可选
     - 要上报的测点的标识符，在此示例中为属性 **power** 的标识符。此处设置的格式必须与服务的数据类型匹配。例如，当质量位被选择时，此处的数据即为 **value** 和 **quality** 。
   * - value
     - Integer
     - 可选
     - 该测点的标识符。在本例中，为参数 **value** 。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为integer时，那么值必须为integer。
   * - quality
     - Integer
     - 可选
     - 该测点的标识符。在本例中，为参数 **quality** 。如上，此处设置的值必须与服务的数据类型匹配。
   * - temp
     - String
     - 可选
     - 要上报的测点的标识符，在此示例中为属性 **temp** 的标识符。如上，此处设置的值必须与服务的数据类型匹配。
   * - branchCurr
     - String
     - 可选
     - 要上报的测点的标识符，在此示例中为属性 **branchCurr** 的标识符。如上，此处设置的值必须与服务的数据类型匹配。
   * - time
     - Timestamp
     - 可选
     - 测点的时间戳。如果为空，时间戳会被设为服务器的时间。
   * - code
     - Integer
     - 必需
     - 结果返回码。200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式


<!--end-->
