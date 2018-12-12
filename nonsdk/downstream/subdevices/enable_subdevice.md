# 子设备启用

用于通知网关下的子设备启用的通知消息，云端使用异步方式发送该消息到网关的TOPIC，通知网关其下属的具体子设备被启用。

下行
- 请求TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/enable`

- 响应TOPIC: `/ext/session/{productKey}/{deviceKey}/combine/enable_reply`

**注意**：TOPIC中的 *productKey* 和 *deviceKey* 为网关的三元组。

## 请求数据格式

```
"id": "123",
"version": "1.0",
"params": [
          {
  "productKey": "xxx",
  "deviceKey": "xxx"
}
]，
"method": "combine.enable"
}

```

## Example of the response message

```
{
	"id": "123",
	"code": 200,
	"data": {}
}

```

## 参数说明​

<table>
  <tr>
    <th>参数</th>
    <th>类型​</th>
    <th>是否必需 </th>
    <th>描述 </th>
  </tr>
  <tr>
    <td>Id</td>
    <td>Long</td>
    <td>可选</td>
    <td>消息ID号，保留值</td>
  </tr>
  <tr>
    <td>version</td>
    <td>String</td>
    <td>必需</td>
    <td>协议版本号，目前协议版本1.0</td>
  </tr>
  <tr>
    <td>params</td>
    <td>Object</td>
    <td>必需</td>
    <td>请求参数 </td>
  </tr>
  <tr>
    <td>productKey</td>
    <td>String</td>
    <td>必需</td>
    <td>子设备的productKey</td>
  </tr>
  <tr>
    <td>deviceKey</td>
    <td>String</td>
    <td>必需</td>
    <td>子设备的deviceKey</td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需</td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需</td>
    <td>200或设备端定义的错误码。200代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>JSON</td>
    <td>可选</td>
    <td>返回的详细信息。JSON格式 </td>
  </tr>
</table>
