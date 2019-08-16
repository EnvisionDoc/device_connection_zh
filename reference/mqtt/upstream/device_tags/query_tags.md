# 获取标签信息

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/tag/query`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/tag/query_reply`

## 请求数据格式


```
{
 "id": "123",
 "version": "1.0",
 "params": {
   "tags": ["tag1", "tag2"]
 },
 "method": "thing.tag.query"
}

```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "data": {
   "tag1": "value1",
   "tag2": "value2"
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
     - 查询标签信息所需的参数。一次最多可查询200个标签
   * - tags
     - List
     - 必需
     - 查询的标签列表。当值为空时，将返回所有的设备标签
   * - method
     - String
     - 必需
     - 请求方法
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行


<!--end-->
