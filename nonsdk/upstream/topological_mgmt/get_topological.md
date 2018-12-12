# 获取子设备拓扑关系

Edge类型的设备， 可以通过该Topic获取该设备和关联的子设备拓扑关系。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/get`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/topo/get_reply`

**注意**: TOPIC中的productKey和deviceKey为网关的三元组。

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
    <th>参数 </th>
    <th>类型 </th>
    <th>是否必需 </th>
    <th>描述 </th>
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
    <td>结果返回码，200代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data </td>
    <td>JSON</td>
    <td>可选 </td>
    <td>返回的详细信息。JSON格式。 </td>
  </tr>
</table>
