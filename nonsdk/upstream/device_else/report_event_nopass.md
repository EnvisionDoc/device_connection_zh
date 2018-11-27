# 设备事件上报 (非透传)

如果使用非透传模式，设备需要按照EnOS Cloud中定义的标准格式(当前为JSON)生成数据，然后上报数据。

- TOPIC: /sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post

- REPLY TOPIC: /sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post_reply

## 请求数据格式

```
{
  "id": "123",
  "version": "1.0",
 "params": {
		"events": {
			"Power": {
				"value": "1.0",
				"quality": "9"
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
    <td>Object</td>
    <td>必需 </td>
    <td>设备事件上报的阐述 </td>
  </tr>
  <tr>
    <td>events</td>
    <td>Object</td>
    <td>必需 </td>
    <td>设备事件 </td>
  </tr>
  <tr>
    <td>power</td>
    <td>String</td>
    <td>可选 </td>
    <td>事件的属性之一 </td>
  </tr>
  <tr>
    <td>value</td>
    <td>String</td>
    <td>可选 </td>
    <td>事件的属性之一 </td>
  </tr>
  <tr>
    <td>quality</td>
    <td>String</td>
    <td>可选 </td>
    <td>事件的属性之一 </td>
  </tr>
  <tr>
    <td>temp</td>
    <td>String</td>
    <td>可选 </td>
    <td>事件的属性之一 </td>
  </tr>
  <tr>
    <td>branchCurr</td>
    <td>String</td>
    <td>可选 </td>
    <td>事件的属性之一 </td>
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
    <td>结果返回码，200 代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>String</td>
    <td>可选 </td>
    <td>返回的详细信息 。JSON 格式 </td>
  </tr>
</table>
