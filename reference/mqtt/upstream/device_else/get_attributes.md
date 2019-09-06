# 获取属性信息

设备端从云端获取属性，云端根据需要查询的属性ID，返回对应的属性内容。当需要查询的属性内容为空时，云端返回设备的所有属性。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/query`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/query_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {
   "attributes": ["attr1", "attr2", "attr3"]
 },
 "method": "thing.attribute.query"
}
```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "data": {
   "attr1": {
       "value": 1.0,
       "value2": 9
     },
   "attr2": 1.02,
   "attr3": [1.02, 2.02, 7.93]
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
   * - params
     - Object
     - 必需
     - 查询属性所需的参数
   * - attributes
     - Array
     - 可选
     - 需要查询的属性的标识符的列表。一次最多可查询200个属性。如果为空，云端会将该设备的所有属性返回
   * - method
     - String
     - 必需
     - 请求方法
   * - attr1
     - String
     - 必需
     - 查询的属性的标识符
   * - value
     - Struct
     - Integer
     - 返回的该属性的详细信息。在本例中，该属性的数据类型为struct，所以为 **value** 与它的值被返回
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式

## 结果返回码

| 返回码 | 错误消息 | 释义|
|---------|---------|---------|
| 1208 | Attribute query data format error | 请求格式不正确 |

<!--end-->
