# 删除子设备拓扑关系

Edge类型的设备， 可以通过该Topic上行请求删除它和子设备之间的拓扑关系。

删除拓扑关系后，子设备利用该网关再次上线时，系统将提示拓扑关系不存在，认证不通过。

上行
- TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/delete
- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/delete_reply

**注意**: TOPIC中的 productKey和 deviceKey为网关的验证码。

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

<table border="1" cellspacing="0" cellpadding="0" width="604">
  <tr>
    <td>参数 </td>
    <td>类型 </td>
    <td>是否必需 </td>
    <td>描述 </td>
  </tr>
  <tr>
    <td>id</td>
    <td>Long</td>
    <td>可选 </td>
    <td>消息ID号，保留值 </td>
  </tr>
  <tr>
    <td>version</td>
    <td>String</td>
    <td>必需 </td>
    <td>协议版本号，目前协议版本1.0</td>
  </tr>
  <tr>
    <td>params</td>
    <td>List</td>
    <td>必需 </td>
    <td>删除拓扑关系的参数 </td>
  </tr>
  <tr>
    <td>deviceKey</td>
    <td>String</td>
    <td>必需 </td>
    <td>子设备的deviceKey </td>
  </tr>
  <tr>
    <td>productKey</td>
    <td>String</td>
    <td>必需 </td>
    <td>子设备的productkey</td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>结果返回码，200代表请求成功执行。 </td>
  </tr>
</table>
