# 批量上报测点信息

批量上报测点适用于以下几种场景：
- 网关设备代理子设备批量上报数据；
- 直连设备批量上报不同时间戳的数据；
- 兼有以上场景；

.. note:: 根据物模型中的输入参数和输出参数来配置下列的参数。如果请求中部分数据发送失败，整个请求全部发送失败，返回第一个出现的错误码。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/measurepoint/post/batch`
- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/measurepoint/post/batch_reply`

## 请求数据格式

``` json
{
    "id":"123",
    "version":"1.0",
    "allowOfflineSubDevice": true,
    "skipInvalidMeasurepoints": true,
    "params":[
        {
            "productKey":"product1",
            "deviceKey":"device1",
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
        {
            "productKey":"product1",
            "deviceKey":"device1",
            "measurepoints":{
                "Power":{
                    "value":2,
                    "quality":9
                },
                "temp":2.02,
                "branchCurr":[
                    "2.02",
                    "3.02",
                    "9.93"
                ]
            },
            "time":123567
        },
        {
            "productKey":"product2",
            "deviceKey":"device2",
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
        }
    ],
    "method":"thing.measurepoint.post.batch"
}
```

## 响应数据格式

``` json
{
    "id":"123",
    "code":200,
    "data":{

    }
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
   * - allowOfflineSubDevice
     - boolean
     - 可选
     - 是否允许给离线子设备发测点数据，默认false。如果该选项为false且请求中存在离线子设备，那么整个请求将会拒绝
   * - skipInvalidMeasurepoints
     - boolean
     - 可选
     - 是否忽略请求中格式无效的测点数据，默认false。如果该选项为false且请求中存在无效测点，那么整个请求将会拒绝
   * - params
     - Object
     - 必需
     - 上报测点所需的参数
   * - productKey
     - String
     - 可选
     - 设备的product key。如需上报具体子设备的数据，需要提供子设备的productKey和deviceKey；如不提供子设备的productKey和deviceKey，则默认使用网关设备topic的这两个参数
   * - deviceKey
     - String
     - 可选
     - 设备的device key。如需上报具体子设备的数据，需要提供子设备的productKey和deviceKey；如不提供子设备的productKey和deviceKey，则默认使用网关设备topic的这两个参数
   * - method
     - String
     - 必需
     - 请求方法
   * - measurepoints
     - Object
     - 必需
     - 需上报的测点的标识符的列表
   * - power
     - String
     - 可选
     - 要上报的测点的标识符，在此示例中为属性 **power** 的标识符。此处设置的格式必须与服务的数据类型匹配。例如，当质量位被选择时，此处的数据即为 **value** 和 **quality** 
   * - value
     - Integer
     - 可选
     - 该测点的标识符。在本例中，为参数 **value** 。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为integer时，那么值必须为integer
   * - quality
     - Integer
     - 可选
     - 该测点的标识符。在本例中，为参数 **quality** 。如上，此处设置的值必须与服务的数据类型匹配
   * - temp
     - String
     - 可选
     - 要上报的测点的标识符，在此示例中为属性 **temp** 的标识符。如上，此处设置的值必须与服务的数据类型匹配
   * - branchCurr
     - String
     - 可选
     - 要上报的测点的标识符，在此示例中为属性 **branchCurr** 的标识符。如上，此处设置的值必须与服务的数据类型匹配
   * - time
     - Timestamp
     - 可选
     - 测点的时间戳。如果为空，时间戳会被设为服务器的时间
   * - code
     - Integer
     - 必需
     - 结果返回码。200代表请求成功执行
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式

## 结果返回码

| 返回码 | 错误信息 | 释义 |
|---------|---------|---------|
| 1204 | Model validate failed | 测点格式不符合模型规定 |
| 1206 | Measurepoint post data format error | JSON请求格式无效 |
| 1212 | No sub-device permission | 子设备不在线或者子设备没有通过指定的网关接入云端 |

<!--end-->
