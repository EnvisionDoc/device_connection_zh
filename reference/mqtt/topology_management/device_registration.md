# 设备注册

## 注册子设备身份

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/device/register`
- 响应TOPIC: `/sys/{productKey}/{deivceKey}/thing/device/register_reply`

### 请求数据格式

``` json
{
    "method":"thing.device.register",
    "id":"1",
    "params":[
        {
            "timezone":"+08:00",
            "deviceKey":"sample_dev_01",
            "productKey":"aVpQQTDp",
            "deviceAttributes":{
                "location":"Shanghai",
                "name":"dev_01"
            },
            "deviceName":{
                "defaultValue":"sample_dev_01",
                "i18nValue":{
                    "en_US":"eng_dev_01",
                    "zh_CN":"中文设备01"
                }
            },
            "deviceDesc":"dev desc"
        },
        {
            "timezone":"+09:00",
            "deviceKey":"sample_dev_02",
            "productKey":"aVpQQTDp",
            "deviceAttributes":{
                "location":"Beijing",
                "name":"dev_01"
            },
            "deviceName":{
                "defaultValue":"sample_dev_02",
                "i18nValue":{
                    "en_US":"eng_dev_02",
                    "zh_CN":"中文设备02"
                }
            },
            "deviceDesc":"dev desc"
        }
    ],
    "version":"1.1"
}
```

### 响应数据格式

``` json
{
    "code":200,
    "data":[
        {
            "deviceSecret":"u2kWIr9eQXcIpXF18X4R",
            "assetId":"LOMCp6V2",
            "deviceKey":"sample_dev_01",
            "productKey":"aVpQQTDp"
        },
        {
            "deviceSecret":"pulapAO2d3Rbopxrw6pz",
            "assetId":"8MGrcj2b",
            "deviceKey":"sample_dev_02",
            "productKey":"aVpQQTDp"
        }
    ],
    "id":"1"
}

```

### 参数说明

.. list-table::
   :widths: auto

   * - 参数
     - 类型
     - 是否必需
     - 说明
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
     - 设备动态注册的参数
   * - deviceAttributes
     - String
     - 可选
     - 设备的属性列
   * - deviceKey
     - String
     - 可选
     - 子设备的deviceKey
   * - deviceName
     - String
     - 可选
     - 子设备的名字
   * - deviceDesc
     - String
     - 可选
     - 子设备的描述
   * - productKey
     - String
     - 必需
     - 子设备的productKey
   * - assetId
     - String
     - 必需
     - 设备的唯一标识符
   * - deviceSecret
     - String
     - 必需
     - 子设备的deviceSecret
   * - method
     - String
     - 必需
     - 请求方法
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式


<!--end-->
