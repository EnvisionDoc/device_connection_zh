# 上报属性信息

设备端上报属性的新值，云端根据上报的请求，更新对应的属性内容。

上行
- 请求TOPIC：`/sys/{productKey}/{deviceKey}/thing/attribute/update`

- 响应TOPIC：`/sys/{productKey}/{deviceKey}/thing/attribute/update_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {
 "attributes": {
 "attr1": {
     "value": 1.0,
     "value2": 9
   },
 "attr2": 1.02,
 "attr3": [1.02, 2.02, 7.93]
 }
 },
 "method": "thing.attribute.update"
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
     - 上报属性所需的参数
   * - attributes
     - Array
     - 可选
     - 需上报的属性的标识符的列表。一次最多可上报200个属性。如果为空，则无上报的属性
   * - method
     - String
     - 必需
     - 请求方法
   * - attr1
     - struct
     - 必需
     - 要上报的属性的标识符，在此示例中为属性 **attr1** 的标识符。此处设置的格式必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为struct时，此处的格式必须与模型中保持一致，为 **value** 和 **value2** 
   * - value
     - String
     - 必需
     - 该属性中的参数名。在本例中，为 **value** 。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为integer时，那么值必须为integer
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式



<!--end-->
