# 下线子设备

上行
- Request TOPIC: /ext/session/{productKey}/{deviceKey}/combine/logout

- Reply TOPIC: /ext/session/{productKey}/{deviceKey}/combine/logout_reply

## 请求数据格式

```
{
 "id": 123,
 "params": {
 "productKey": "xxxxx",
 "deviceKey": "xxxxx"
 }
}

```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "message": "success",
 "data": {
      "productKey": "xxxxx",
      "deviceKey": "xxxxx"
  }
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
    <td>可选 </td>
    <td>消息ID号，保留值 </td>
  </tr>
  <tr>
    <td>params</td>
    <td>List</td>
    <td>必需 </td>
    <td>子设备下线的参数 </td>
  </tr>
  <tr>
    <td>deviceKey</td>
    <td>String</td>
    <td>必需 </td>
    <td>子设备的deviceKey</td>
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
    <td>message</td>
    <td>String</td>
    <td>可选 </td>
    <td>结果返回信息 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>String</td>
    <td>可选 </td>
    <td>返回的详细信息。JSON格式 </td>
  </tr>
</table>
