# 上报属性信息

第三方系统上报属性的历史数据，EnOS根据上报的数据，更新对应的属性内容。

上行
- 请求TOPIC：`sys/${productKey}/integration/attributes/post`

- 响应TOPIC：`sys/${productKey}/integration/attributes/post_reply`

## 请求数据格式

```JSON
{
    "id":"123",
    "version":"1.0",
    "params":[
        {
            "deviceKey":"device1",
            "attributes":{
                "attr1":{
                    "key1":"val1",
                    "key2":"val2",
                }
            }
        },
        {
            "deviceKey":"device2",
            "attributes":{
                "attr2":2.3
            }
        }
    ],
    "method":"integration.attribute.post"
}
```

## 响应数据格式

```JSON
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
     - 上报属性所需的参数
   * - attributes
     - Array
     - 可选
     - 需上报的属性的标识符的列表。一次最多可上报200个属性。如果为空，则无上报的属性<!--离线数据一个包最多上传200个属性吗？ -->
   * - attr1
     - struct
     - 必需
     - 要上报的属性的标识符，在此示例中为属性 **attr1**。此处设置的格式必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为struct时，此处的格式必须与模型中保持一致，本示例中为 **key1** 和 **key2** 
   * - key1
     - String
     - 必需
     - 该属性中的参数名
   * - key2
     - String
     - 必需
     - 该属性中的参数名
   * - method
     - String
     - 必需
     - 请求方法
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式

## 结果返回码

| 返回码 | 错误信息 | 释义|
|---------|---------|---------|
| 1204 | Model validate failed | 数据格式不符合模型定义 |
| 1257 | Device not found | 设备不存在 |

<!--end-->
