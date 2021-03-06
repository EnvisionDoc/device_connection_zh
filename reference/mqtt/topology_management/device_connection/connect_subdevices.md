# 上线子设备

在子设备上线前，需要确保子设备身份已经在EnOS Cloud中注册，并在Edge中添加拓扑关系。云端需要根据拓扑关系对子设备进行身份校验，以确定子设备具有使用网关通道的能力，才会上线该子设备。

上行
- 请求TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/login`

- 响应TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/login_reply`

## 请求数据格式

``` json
{
    "id":"123",
    "params":{
        "productKey":"123",
        "deviceKey":"test",
        "clientId":"123",
        "timestamp":"123",
        "signMethod":"hmacmd5",
        "sign":"xxxxxx",
        "cleanSession":"true"
    }
}
```

## 响应数据格式

```
{
 "id":"123",
 "code":200,
 "message":"success"
 "data": {
      "productKey": "123",
      "deviceKey": "test"
  }
}

```

所有发往EnOS Cloud的参数都会被加密，除了sign和signmethod之外。EnOS Cloud会将参数按照字母顺序排序，然后将参数和值依次拼接（无拼接符号）。对加签内容，需使用signMethod指定的加签算法进行加签。

例如，在如下request请求中，对params中的参数按照字母顺序依次拼接后进行加签。
`sign=uppercase(hmacsha1( cleanSessiontrueclientId123deviceNametestproductKey123timestamp123{deviceSecret}))`


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
     - 子设备上线的参数
   * - deviceKey
     - String
     - 必需
     - 子设备的deviceKey
   * - productKey
     - String
     - 必需
     - 子设备的productKey
   * - sign
     - String
     - 必需
     - 子设备签名，规则与网关相同
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
     - 设备端标识，可以为productKey或deviceName。
   * - cleanSession
     - String
     - 必需
     - 值为true，true代表清理所有子设备离线消息，即QoS1的所有未接收内容
   * - message
     - String
     - 必需
     - 结果返回信息
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式

.. note:: 网关下同时在线的子设备数目不能超过200，超过后，新的子设备上线请求将被拒绝。

## 结果返回码

| 返回码 | 错误消息 | 释义 |
|---------|---------|---------|
| 705 | It failed to query device, not existed this device | 子设备不存在 |
| 723 | Device is disable | 子设备被禁用 |
| 770 | Dynamic activate is not allowed | 该产品未启用动态激活 |
| 771 | Sub device cannot connect to mqtt broker directly | 子设备不能与EnOS Cloud直连 |
| 740 | Sub device not belong the gateway | 该设备并非该网关的子设备 |
| 742 | Sign check failed | Hash签名验证失败 |
| 746 | The device must login by ssl | 该产品启用了证书双向认证 |

<!--end-->
