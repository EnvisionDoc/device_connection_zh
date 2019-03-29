# 删除属性

设备端删除设备属性的请求，云端根据上报的请求，删除对应的属性值。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/delete`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/delete_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {
   "attributes": ["attr1", "attr2", "attr3"]
 },
 "method": "thing.attribute.delete"
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
     - 删除属性所需的参数
   * - attributes
     - Array
     - 可选
     - 需要删除的属性的标识符的列表。一次最多可删除200个属性。如果为空，则不会删除任何属性。
   * - method
     - String
     - 必需
     - 请求方法
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式



<!--end-->
