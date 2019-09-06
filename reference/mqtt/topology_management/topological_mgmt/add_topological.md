# 添加子设备拓扑关系

Edge类型的设备， 可以通过该Topic上行请求添加它和子设备之间的拓扑关系。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/add`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/add_reply`

.. note:: TOPIC中的 productKey和 deviceKey为网关的三元组。


## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "deviceKey": "deviceKey1234",
 "productKey": "1234556554",
 "sign": "xxxxxx",
 "signmethod": "hmacSha1",
 "timestamp": "1524448722000",
 "clientId": "xxxxxx"
 }
 ],
 "method": "thing.topo.add"
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
     - 添加拓扑关系的参数
   * - deviceKey
     - String
     - 必需
     - 子设备的deviceKey
   * - productKey
     - String
     - 必需
     - 子设备的productkey
   * - sign
     - String
     - 必需
     - 签名
   * - signmethod
     - String
     - 必需
     - 签名方法，支持hmacSha1
   * - timestamp
     - String
     - 必需
     - 时间戳
   * - clientId
     - String
     - 必需
     - 本地标记。可以为productKey或deviceKey
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行



所有发往EnOS Cloud 的参数都会被加密，除了sign和signmethod之外。EnOS Cloud会将参数按照字母顺序排序，然后将参数和值依次拼接（无拼接符号）。对加签内容，需使用signMethod指定的加签算法进行加签。

例如，在如下request请求中，对params中的参数按照字母顺序依次拼接后进行加签。

sign=uppercase(hmacsha1( clientId123deviceKeytestproductKey123timestamp1524448722000{deviceSecret}))

## 结果返回码

| 返回码 | 错误消息 | 释义 |
|---------|---------|---------|
| 1255 | add topo failure, \[details\] | 部分或者全部子设备添加到网关失败，具体哪些子设备的topo添加失败，需要查看 具体错误信息 [#f1]_ |

.. [#f1] 具体错误包括：
  - 当前添加到的设备不是网关类型 
  - 提供的子设备不存在 
  - 超过了允许的最大子设备数量限制
  - 提供的子设备中包含网关 
  - 提供的子设备中，有些设备已经属于其他网关的子设备

<!--end-->
