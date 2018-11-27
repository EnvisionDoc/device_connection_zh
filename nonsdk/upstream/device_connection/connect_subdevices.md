# 子设备上线

- Request TOPIC: /ext/session/{productKey}/{deviceKey}/combine/login

-   Reply TOPIC: /ext/session/{productKey}/{deviceKey}/combine/login_reply

## 请求数据格式

```
{
 "id": "123",
 "params": {
 "productKey": "123",
 "deviceKey": "test",
 "clientId": "123",
 "timestamp": "123",
 "signMethod": "hmacmd5",
 "sign": "xxxxxx",
 "cleanSession": "true"
 }
}

```

## 响应数据格式

```
{
 "id":"123",
 "code":200,
 "message":"success"
 "data": {
      "productKey": "123",
      "deviceKey": "test"
  }
}

```

所有发往EnOS Cloud 的参数都会被加密，除了sign 和signmethod之外。 EnOS Cloud会将参数按照字母顺序排序，然后将参数和值依次拼接（无拼接符号）。对加签内容，需使用signMethod指定的加签算法进行加签。

例如，在如下request请求中，对 params中的参数按照字母顺序依次拼接后进行加签。

sign= uppercase(hmac_sha1(deviceSecret, cleanSessiontrueclientId123deviceNametestproductKey123timestamp123))


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
    <td>Parameters used for connecting sub-device   to EnOS Cloud.</td>
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
    <td>sign</td>
    <td>String</td>
    <td>必需 </td>
    <td>子设备签名，规则与网关相同 </td>
  </tr>
  <tr>
    <td>signmethod</td>
    <td>String</td>
    <td>必需 </td>
    <td>签名方法，支持hmacSha1，hmacSha256，hmacMd5，Sha256</td>
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
    <td>设备端标识，可以为productKey或deviceName。 </td>
  </tr>
  <tr>
    <td>cleanSession </td>
    <td>String </td>
    <td>必需 </td>
    <td>值为true，true代表清理所有子设备离线消息，即QoS1的所有未接收内容 </td>
  </tr>
  <tr>
    <td>message </td>
    <td>String</td>
    <td>必需 </td>
    <td>结果返回信息 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>String </td>
    <td>可选 </td>
    <td>返回的详细信息 。JSON 格式 </td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>结果返回码，200 代表请求成功执行。 </td>
  </tr>
</table>
