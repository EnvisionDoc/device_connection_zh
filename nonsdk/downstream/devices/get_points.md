# 测点信息获取

**注意**：根据物模型中的输入参数和输出参数来配置下列的参数。

设备收到获取设备测点获取指令之后，通过reply消息向云端返回获取的测点信息。可以通过数据流转获取返回的测点信息。

- TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/get

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/get_reply

## 请求数据格式

```
{
	"id": "123",
	"version": "1.0",
	"params": ["power", "temp"],
	"method": "thing.service.measurepoint.get"
}

```

## 响应数据格式

```
{
	"id": "123",
	"code": 200,
	"data": {
		"power": "on",
		"temp": "23"
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
    <td>version</td>
    <td>String</td>
    <td>必需 </td>
    <td>协议版本号，目前协议版本1.0</td>
  </tr>
  <tr>
    <td>params</td>
    <td>List</td>
    <td>必需 </td>
    <td>获取测点信息的参数 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>power</td>
    <td>String</td>
    <td>可选 </td>
    <td>测点的属性之一 </td>
  </tr>
  <tr>
    <td>temp</td>
    <td>String</td>
    <td>可选 </td>
    <td>测点的属性之一 </td>
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
