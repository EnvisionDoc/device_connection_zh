# 子设备拓扑关系的获取

Edge类型的设备， 可以通过该Topic获取该设备和关联的子设备拓扑关系。

- Request TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/get

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/get_reply

**注意**: TOPIC中的 productKey和 deviceKey为网关的验证码。

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

<table>
  <tr>
    <td>参数 </td>
    <td>类型 </td>
    <td>是否必需 </td>
    <td>描述 </td>
  </tr>
  <tr>
    <td>id</td>
    <td>Long</td>
    <td>必需 </td>
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
    <td>Object</td>
    <td>可选 </td>
    <td>获取拓扑关系的参数 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
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
    <td>子设备的productKey</td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>结果返回码，200 代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data </td>
    <td>String </td>
    <td>可选 </td>
    <td>返回的详细信息 。JSON 格式。 </td>
  </tr>
</table>