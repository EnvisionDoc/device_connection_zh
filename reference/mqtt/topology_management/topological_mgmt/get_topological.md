# 获取子设备拓扑关系

Edge类型的设备， 可以通过该Topic获取该设备和关联的子设备拓扑关系。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/get`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/get_reply`

.. note:: TOPIC中的productKey和deviceKey为网关的三元组。

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {},
 "method": "thing.topo.get"
}

```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "data": [
 {
 "deviceKey": "deviceKey1234",
 "productKey": "1234556554"
 }
 ]
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
     - 必需
     - 消息ID号，保留值
   * - version
     - String
     - 必需
     - 协议版本号，目前协议版本1.0
   * - params
     - Object
     - 可选
     - 获取拓扑关系的参数
   * - method
     - String
     - 必需
     - 请求方法
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
   * - data
     - JSON
     - 可选
     - 返回的详细信息。JSON格式

## 结果返回码

| 返回码 | 错误消息 | 释义 |
|---------|---------|---------|
| 1200 | Parse error, \[details\] | 解析错误，具体原因需查看\[details\]，最有可能的原因是当前发出请求的设备不是网关类型 |

<!--end-->
