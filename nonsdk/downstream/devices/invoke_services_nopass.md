# 调用设备服务 (非透传)

设备端通过服务调用topic 响应设备服务调用请求，云端采用同步或异步的方式推送该消息，设备端可以利用reply topic响应该消息。

下行
- TOPIC: /sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}_reply

## 请求数据格式

```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"Power": "on",
		"WindState": 2
	},
	"method": "thing.service.{tsl.service.identifier}"
}
```

## 响应数据格式

```
"id": "123",
"code": 200,
"data": {}
}

```

## 参数说明​

<table>
  <tr>
    <td>参数 </td>
    <td>类型​</td>
    <td>是否必需 </td>
    <td>描述 </td>
  </tr>
  <tr>
    <td>Id</td>
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
    <td>调用设备服务的参数 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>Power</td>
    <td>String</td>
    <td>可选 </td>
    <td>服务的属性之一 </td>
  </tr>
  <tr>
    <td>WindState</td>
    <td>String</td>
    <td>可选 </td>
    <td>服务的属性之一 </td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>返回设备端定义的错误码。200代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>String</td>
    <td>可选 </td>
    <td>返回的详细信息 。JSON格式 </td>
  </tr>
</table>

**注意**：``tsl.service.identifier ``为TSL模板中事件的描述符。
