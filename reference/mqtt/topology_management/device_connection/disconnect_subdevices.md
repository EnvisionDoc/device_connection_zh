# 下线子设备

上行
- 请求TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/logout`

- 响应TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/logout_reply`

## 请求数据格式

```
{
 "id": 123,
 "params": {
 "productKey": "xxxxx",
 "deviceKey": "xxxxx"
 }
}

```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "message": "success",
 "data": {
      "productKey": "xxxxx",
      "deviceKey": "xxxxx"
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
   * - params
     - List
     - 必需
     - 子设备下线的参数
   * - deviceKey
     - String
     - 必需
     - 子设备的deviceKey
   * - productKey
     - String
     - 必需
     - 子设备的productKey
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行
   * - message
     - String
     - 可选
     - 结果返回信息
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式



<!--end-->
