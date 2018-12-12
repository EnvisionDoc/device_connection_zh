# 设备事件上报 (非透传)

如果使用非透传模式，设备需要按照EnOS Cloud中定义的标准格式(当前为JSON)生成数据，然后上报数据。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post_reply`

## 请求数据格式

```
{
  "id": "123",
  "version": "1.0",
 "params": {
		"events": {
			"Power": {
				"value": 1.0,
				"quality": 9
			},
			"temp": 1.02,
			"branchCurr": [
				"1.02", "2.02", "7.93"
			]
		},
		"time": 123456
	},
  "method": "thing.event.{tsl.event.identifier}.post"
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
    <th>参数 </th>
    <th>类型 </th>
    <th>是否必需 </th>
    <th>描述 </th>
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
    <td>Object</td>
    <td>必需 </td>
    <td>设备事件上报的参数 </td>
  </tr>
  <tr>
    <td>events</td>
    <td>Object</td>
    <td>必需 </td>
    <td>需上报的事件的标识符的列表。 </td>
  </tr>
  <tr>
    <td>power</td>
    <td>-</td>
    <td>可选 </td>
    <td>要上报的事件的标识符。在本例中，为参数<strong>power</strong>。此处设置的格式必须与服务的数据类型匹配。例如,当此参数的数据类型在模型中设置为struct时，此处的格式必须与模型中保持一致，为<strong>value</strong>和<strong>value2</strong>。 </td>
  </tr>
  <tr>
    <td>value</td>
    <td>Integer</td>
    <td>可选 </td>
    <td>该事件的标识符。在本例中，为参数<strong>value</strong>。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为integer时，那么值必须为integer。</td>
  </tr>
  <tr>
    <td>quality</td>
    <td>Integer</td>
    <td>可选 </td>
    <td>该事件出参的名称。在本例中，为参数<strong>quality</strong>。如上，此处设置的格式必须与服务的数据类型匹配。</td>
  </tr>
  <tr>
    <td>temp</td>
    <td>Integer</td>
    <td>可选 </td>
    <td>要上报的事件的标识符。在本例中，为参数<strong>temp</strong>。如上，此处设置的格式必须与服务的数据类型匹配。 </td>
  </tr>
  <tr>
    <td>branchCurr</td>
    <td>Array</td>
    <td>可选 </td>
    <td>要上报的事件的标识符。在本例中，为参数<strong>branchCurr</strong>。如上，此处设置的格式必须与服务的数据类型匹配。 </td>
  </tr>
  <tr>
    <td>time</td>
    <td>String</td>
    <td>可选 </td>
    <td>测点的时间戳。如果为空，时间戳会被设为服务器的时间。 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>可选 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>结果返回码，200代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>String</td>
    <td>可选 </td>
    <td>返回的详细信息 。JSON格式 </td>
  </tr>
</table>
