# 上报测点信息

**注意**：根据物模型中的输入参数和输出参数来配置下列的参数。

上行
- Request TOPIC: /sys/{productKey}/{deviceKey}/thing/measurepoint/post

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/measurepoint/post_reply

## 请求数据格式

```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"measurepoints": {
			"Power": {
				"value": "1.0",
				"quality": "9"
			},
			"temp": 1.02,
			"branchCurr": [
				"1.02", "2.02", "7.93"
			]
		}
		"time": 123456
	}
},
"method": "thing.measurepoint.post"
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
    <td>参数</td>
    <td>类型</td>
    <td>是否必需</td>
    <td>描述</td>
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
    <td>上报 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>measurepoints</td>
    <td>Object</td>
    <td>必需 </td>
    <td>设备测点 </td>
  </tr>
  <tr>
    <td>power</td>
    <td>String</td>
    <td>可选 </td>
    <td>测点的属性之一 </td>
  </tr>
  <tr>
    <td>value</td>
    <td>String</td>
    <td>Optioanl</td>
    <td>测点的属性之一 </td>
  </tr>
  <tr>
    <td>quality</td>
    <td>Strign</td>
    <td>Optioanl</td>
    <td>测点的属性之一 </td>
  </tr>
  <tr>
    <td>temp</td>
    <td>String</td>
    <td>Optioanl</td>
    <td>测点的属性之一 </td>
  </tr>
  <tr>
    <td>branchCurr</td>
    <td>String</td>
    <td>Optioanl</td>
    <td>测点的属性之一 </td>
  </tr>
  <tr>
    <td>time</td>
    <td>Timestamp</td>
    <td>Optioanl</td>
    <td>测点的时间戳。如果为空，时间戳会被设为服务器的时间。 </td>
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
