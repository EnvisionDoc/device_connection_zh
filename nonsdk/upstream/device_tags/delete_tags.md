# 删除标签信息

上行
- 请求TOPIC：`/sys/{productKey}/{deviceName}/thing/tag/delete`

- 响应TOPIC：`/sys/{productKey}/{deviceName}/thing/tag/delete_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {
    "tags": ["tag1", "tag2"]
 },
 "method": "thing.tag.delete"
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
     - List
     - 必需
     - 删除标签信息所需参数
   * - tags
     - List
     - 必需
     - 标签标识符。如值为空，则不删除任何标签。
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
