# 设备注册

## 注册子设备身份

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/device/register`

- 响应TOPIC: `/sys/{productKey}/{deivceKey}/thing/device/register_reply`

### 请求数据格式

```
"id": "123",
"version": "1.0",
"params": [
{
"productKey": "1234556554",
"deviceAttributes": {
"color":"red"
},
"deviceKey" : "deviceKey1234",
"deviceName": "deviceName1234",
"deviceDesc": "deviceDesc1234"
}
],
"method": "thing.device.register"
}

```

### 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "data": [
 {
 "assetId": "12344",
 "productKey": "1234556554",
 "deviceKey": "deviceKey1234",
 "deviceSecret": "xxxxxx"
 }
 ]
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
   * - color
     - String
     - 可选
     - 设备的属性
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
     - 结果返回码，200代表请求成功执行。
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式。


<!--end-->
