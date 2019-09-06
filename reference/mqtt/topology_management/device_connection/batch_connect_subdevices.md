# 批量上线子设备

## 开始前准备

1. 需要确保子设备身份已经在EnOS Cloud中注册。

2. 在网关设备中添加拓扑关系。

## Topics

上行：

- 请求TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/login/batch`
- 响应TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/login/batch_reply`

## 请求数据格式

``` json
{
    "method":"combine.login.batch",
    "id":"1",
    "params":{
        "clientId":"Lx5Q1X6M",
        "subDevices":[
            {
                "sign":"c14fc21231e6c44849683ccfb7a2089895a278b37a30c33ccb58d3b8690d16e1",
                "deviceKey":"xGKFwfkEXz",
                "productKey":"x4jwTsoz"
            },
            {
                "sign":"1fbf0cd0903a9405d567fe5909a1aae75012df5465211db367c7666c0ae33bf2",
                "deviceKey":"cS9MdVJwO9",
                "productKey":"x4jwTsoz"
            },
            {
                "sign":"480869163c85777c9fcb9fe53340fb13d38ef19e644df82927fcd2393b3819c4",
                "deviceKey":"viWaBURIDm",
                "productKey":"x4jwTsoz"
            }
        ],
        "signMethod":"sha256",
        "timestamp":"1564988853275"
    },
    "version":"1.1"
}
```

## 响应数据格式 （分三种情况）

##### a) 子设备全部登入成功 
``` json
{
    "id":"1",
    "code":200,
    "data":{
        "loginedSubDevices":[
            {
                "assetId":"LhBLfxpG",
                "deviceKey":"cS9MdVJwO9",
                "productKey":"x4jwTsoz"
            },
            {
                "assetId":"kAHwVbyl",
                "deviceKey":"viWaBURIDm",
                "productKey":"x4jwTsoz"
            },
            {
                "assetId":"AkHJvoqN",
                "deviceKey":"xGKFwfkEXz",
                "productKey":"x4jwTsoz"
            }
        ],
        "failedSubDevices":[]
    },
    "message":""
}
```

##### b) 部分子设备登入失败
``` json
{
    "id":"1",
    "code":723,
    "data":{
        "loginedSubDevices":[
            {
                "assetId":"LhBLfxpG",
                "deviceKey":"cS9MdVJwO9",
                "productKey":"x4jwTsoz"
            },
            {
                "assetId":"kAHwVbyl",
                "deviceKey":"viWaBURIDm",
                "productKey":"x4jwTsoz"
            }
        ],
        "failedSubDevices":[
            {
                "deviceKey":"xGKFwfkEXz",
                "productKey":"x4jwTsoz"
            }
        ]
    },
    "message":"x4jwTsoz&xGKFwfkEXz has error: Device is disable;"
}
```

##### c) 请求格式存在错误
``` json
{
    "id":"1",
    "code":1220,
    "data":{
        "loginedSubDevices":[],
        "failedSubDevices":[]
    },
    "message":"payload format error, ..."
}
```

## 正确处理响应

当相应数据的code为200时，这表明子设备全部登入成功。否则，这表明子设备全部或者部分登入失败。当请求存则格式错误（或者其他内部服务错误）时，响应中loginedSubDevices和failedSubDevices将为空。因此，当响应中code不为200时，用户不能简单地通过failedSubDevices判断哪些子设备登入失败。因此，响应的正确处理方式应为：

``` java
if (code == 200) {
  // 所有的子设备登入成功
} else {
  if (loginedSubDevices.isEmpty() && failedSubDevices.isEmpty()) {
    // 存在格式错误或内部服务错误
  } else {
    if (loginedSubDevices.isNotEmpty()) {
      // 处理登入成功的子设备
    }
    if (failedSubDevices.isNotEmpty()) {
      // 处理登入失败的子设备
    }
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
     - 设备端标识，可以为任意标志（建议用网关的productKey或者deviceKey）。
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
     - 必须
     - 返回的详细信息。JSON格式
   * - loginedSubDevices
     - Array
     - 必须
     - 登入成功的子设备
   * - failedSubDevices
     - Array
     - 必须
     - 登入失败的子设备

.. note:: 网关下同时在线的子设备数目不能超过200，超过后，新的子设备上线请求将被拒绝。

## 结果返回码

| 返回码 | 错误信息 | 释义 |
|---------|---------|---------|
| 705 | It failed to query device, not existed this device | 子设备不存在 |
| 723 | Device is disable | 子设备被禁用 |
| 770 | Dynamic activate is not allowed | 该产品未启用动态激活 |
| 771 | Sub device cannot connect to mqtt broker directly | 子设备不能与EnOS Cloud直连 |
| 740 | Sub device not belong the gateway | 该设备并非该网关的子设备 |
| 742 | Sign check failed | Hash签名验证失败 |
| 746 | The device must login by ssl | 该产品启用了证书双向认证 |

<!--end-->
