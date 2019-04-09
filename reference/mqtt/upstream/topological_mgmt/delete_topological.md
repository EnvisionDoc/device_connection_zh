# 删除子设备拓扑关系

Edge类型的设备， 可以通过该Topic上行请求删除它和子设备之间的拓扑关系。

删除拓扑关系后，子设备利用该网关再次上线时，系统将提示拓扑关系不存在，认证不通过。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/delete`
- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/delete_reply`

.. note:: TOPIC中的 productKey和 deviceKey为网关的验证码。

## 请求数据格式

```
"id": "123",
 "version": "1.0",
 "params": [
 {
 "deviceKey": "deviceKey1234",
 "productKey": "1234556554"
 }
 ],
 "method": "thing.topo.delete"
}

```

## 响应数据格式

```
{
 "id
": "123",
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
     - 删除拓扑关系的参数
   * - deviceKey
     - String
     - 必需
     - 子设备的deviceKey
   * - productKey
     - String
     - 必需
     - 子设备的productkey
   * - method
     - String
     - 必需
     - 请求方法
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行。

<!--end-->