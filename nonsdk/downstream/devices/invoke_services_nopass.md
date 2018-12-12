# 调用设备服务 (非透传)

设备端通过服务调用TOPIC响应设备服务调用请求，云端采用同步或异步的方式推送该消息，设备端可以利用reply topic响应该消息。

下行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}_reply`

**注意**：``tsl.service.identifier ``为TSL模板中事件的描述符。


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
    <th>参数 </th>
    <th>类型​</th>
    <th>是否必需 </th>
    <th>描述 </th>
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
    <td>要调用的服务的标识符，在此示例中为服务<strong>Power</strong>的标识符。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为string时，此处的值必须为string。 </td>
  </tr>
  <tr>
    <td>WindState</td>
    <td>integer</td>
    <td>可选 </td>
    <td>要调用的服务的标识符，在此示例中为服务<strong>Windstate</strong>的标识符。如上，此处设置的值必须与服务的数据类型匹配。</td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>200或设备端定义的错误码。200代表请求成功执行。</td>
  </tr>
  <tr>
    <td>data</td>
    <td>JSON</td>
    <td>可选 </td>
    <td>返回的详细信息。JSON格式 </td>
  </tr>
</table>
