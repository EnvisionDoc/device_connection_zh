# 历史事件集成

如果使用非透传模式，第三方系统需要按照EnOS Cloud中定义的标准格式(当前为JSON)生成数据，然后上报数据。

上行
- 请求TOPIC: `sys/${productKey}/integration/event/post`

- 响应TOPIC: `sys/${productKey}/integration/event/post_reply`

## 请求数据格式
```json
{
    "id":"123",
    "version":"1.0",
    "params":[
        {
            "deviceKey":"device1",
            "events":{
                "eventId1":{
                    "alarm":9,
                    "level":3
                }
            },
            "time":123456
        },
        {
            "deviceKey":"device2",
            "events":{
                "eventId2":{
                    "result":"ok"
                }
            },
            "time":123456
        }
    ],
    "method":"integration.event.post"
}

```

## 响应数据格式

```json
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
   * - deviceKey
     - String
     - 必需
     - 设备的device key
   * - events
     - Object
     - 必需
     - 需上报的事件的标识符的列表
   * - eventId1
     - String
     - 必需
     - 需上报的事件的标识符。在本例中，为参数 **eventId1**和**eventId2** 。此处设置的格式必须与服务的数据类型匹配。例如,当此参数的数据类型在模型中设置为struct时，此处的格式必须与模型中保持一致，此示例中为 **alarm** 和 **level** 
   * - alarm
     - Integer
     - 可选
     - 要上报的事件的成员的标识符。在本例中，为参数 **alarm** 。其值的格式必须与服务的数据类型匹配。例如,当此参数的数据类型在模型中设置为int时，其值格式必须与模型中保持一致，为 **9** 
   * - level
     - Integer
     - 可选
     - 要上报的事件的成员的标识符。在本例中，为参数 **level** 。其值的格式必须与服务的数据类型匹配。例如,当此参数的数据类型在模型中设置为int时，其值格式必须与模型中保持一致，为 **3**
   * - result
     - String
     - 可选
     - 要上报的事件的成员的标识符。在本例中，为参数 **result** 。其值的格式必须与服务的数据类型匹配。例如,当此参数的数据类型在模型中设置为string时，其值格式必须与模型中保持一致，为 **OK**
   * - time
     - String
     - 可选
     - 事件的时间戳。如果为空，时间戳会被设为服务器的时间
   * - method
     - String
     - 可选
     - 请求方法
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行
   * - data
     - String
     - 可选
     - 返回的详细信息 。JSON格式

## 结果返回码

| 返回码 | 错误信息 | 释义|
|---------|---------|---------|
| 1204 | Model validate failed | 数据格式不符合模型定义 |
| 1257 | Device not found | 设备不存在 |

<!--end-->
