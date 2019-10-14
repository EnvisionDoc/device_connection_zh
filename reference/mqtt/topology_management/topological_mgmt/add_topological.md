# 添加子设备拓扑关系

网关类型的设备，可以通过该Topic上行请求添加它和子设备之间的拓扑关系。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/add`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/add_reply`

.. note:: TOPIC中的`productKey`和`deviceKey`为网关设备的product key和device key。


## 请求数据格式

```json
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

```json
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
     - 签名，生成方法见下文
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


生成`sign`需要使用`params`结构体中，除去`sign`和`signmethod`之外其他的键值对。其步骤如下：

1. 将除了`sign`和`signmethod`之外其他的键值对，按照键的首字母顺序，以“键名+值”“键名+值”“键名+值”的方式拼接成一个字符串（无需“+”号或者任何其他表示拼接的符号）

  例如，在上述请求中，`params`中被抽取的字段应拼接成如下字符串：
  `clientIdxxxxxxdeviceKeydeviceKey1234productKey1234556554timestamp1524448722000`

2. 将该字符串与子设备的device secret拼接，再使用`signmethod`中规定的方法进行签名运算，并将所有英文字符转换为大写字母。假定子设备的device secret为xyz123,则其运算表达式如下：

  sign=uppercase(hmacsha1(clientId123deviceKeytestproductKey123timestamp1524448722000xyz123))

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
