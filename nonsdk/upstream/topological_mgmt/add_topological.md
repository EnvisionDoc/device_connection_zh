# 添加子设备拓扑关系

Edge类型的设备， 可以通过该Topic上行请求添加它和子设备之间的拓扑关系。

上行
- Request TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/add

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/add_reply

**注意**: TOPIC中的 productKey和 deviceKey为网关的验证码。


## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "deviceKey": "deviceKey1234",
 "productKey": "1234556554",
 "sign": "xxxxxx",
 "signmethod": "hmacSha1",
 "timestamp": "1524448722000",
 "clientId": "xxxxxx"
 }
 ],
 "method": "thing.topo.add"
}
```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "data": {}
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
    <td>version</td>
    <td>String</td>
    <td>必需 </td>
    <td>协议版本号，目前协议版本1.0</td>
  </tr>
  <tr>
    <td>params</td>
    <td>List</td>
    <td>必需 </td>
    <td>添加拓扑关系的参数 </td>
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
    <td>sign</td>
    <td>String</td>
    <td>必需 </td>
    <td>签名 </td>
  </tr>
  <tr>
    <td>signmethod</td>
    <td>String</td>
    <td>必需 </td>
    <td>签名方法，支持hmacSha1，hmacSha256，hmacMd5，Sha256。 </td>
  </tr>
  <tr>
    <td>timestamp</td>
    <td>String</td>
    <td>必需 </td>
    <td>时间戳 </td>
  </tr>
  <tr>
    <td>clientId</td>
    <td>String</td>
    <td>必需 </td>
    <td>本地标记。可以为productKey或deviceKey。 </td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>结果返回码，200代表请求成功执行。 </td>
  </tr>
</table>

所有发往EnOS Cloud 的参数都会被加密，除了sign 和signmethod之外。 EnOS Cloud会将参数按照字母顺序排序，然后将参数和值依次拼接（无拼接符号）。对加签内容，需使用signMethod指定的加签算法进行加签。

例如，在如下request请求中，对 params中的参数按照字母顺序依次拼接后进行加签。

sign= uppercase(hmacsha1(deviceSecret, clientId123deviceKeytestproductKey123timestamp1524448722000))
