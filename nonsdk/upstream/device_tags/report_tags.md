# 上报标签信息

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/tag/update`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/tag/update_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "tagKey": "Temperature",
 "tagValue": "36.8"
 }
 ],
 "method": "thing.tag.update"
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
     - 上报标签信息所需的参数。一次最多可上报200个标签。
   * - method
     - String
     - 必需
     - 请求方法
   * - tagKey
     - String
     - 必需
     - 标签的名字
       长度不超过100字节
       仅支持字符集 [a-z,  A-Z, 0-9]和下划线。
       名字的首字母必需是字母或下划线
   * - tagValue
     - String
     - 必需
     - 标签的值
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息 。JSON格式


<!--end-->
