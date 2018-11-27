# 设备注册

## 子设备身份注册

- Request TOPIC: /sys/{productKey}/{deviceKey}/thing/device/register

- Reply TOPIC: /sys/{productKey}/{deivceKey}/thing/device/register_reply

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

<table>
  <tr>
    <td>参数 </td>
    <td>类型 </td>
    <td>是否必需 </td>
    <td>说明 </td>
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
    <td>设备动态注册的参数 </td>
  </tr>
  <tr>
    <td>deviceAttributes</td>
    <td>String</td>
    <td>可选 </td>
    <td>设备的属性列 </td>
  </tr>
  <tr>
    <td>color</td>
    <td>String</td>
    <td>可选 </td>
    <td>设备的属性 </td>
  </tr>
  <tr>
    <td>deviceKey</td>
    <td>String</td>
    <td>可选 </td>
    <td>子设备的deviceKey</td>
  </tr>
  <tr>
    <td>deviceName</td>
    <td>String</td>
    <td>可选 </td>
    <td>子设备的名字 </td>
  </tr>
  <tr>
    <td>deviceDesc</td>
    <td>String</td>
    <td>可选 </td>
    <td>子设备的描述 </td>
  </tr>
  <tr>
    <td>productKey</td>
    <td>String</td>
    <td>必需 </td>
    <td>子设备的productKey</td>
  </tr>
  <tr>
    <td>assetId</td>
    <td>String</td>
    <td>必需 </td>
    <td>设备的唯一标识Id</td>
  </tr>
  <tr>
    <td>deviceSecret</td>
    <td>String </td>
    <td>必需 </td>
    <td>子设备的deviceSecret</td>
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
    <td>结果返回码，200 代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>String</td>
    <td>可选 </td>
    <td>返回的详细信息 。JSON 格式。 </td>
  </tr>
</table>
