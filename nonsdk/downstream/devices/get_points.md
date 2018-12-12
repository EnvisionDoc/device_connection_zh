# 获取测点信息

设备收到获取设备测点获取指令之后，通过reply消息向云端返回获取的测点信息。可以通过数据流转获取返回的测点信息。

**注意**：根据物模型中的输入参数和输出参数来配置下列的参数。

下行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/measurepoint/get`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/service/measurepoint/get_reply`

## 请求数据格式<request>

```
{
	"id": "123",
	"version": "1.0",
	"params": ["power", "temp"],
	"method": "thing.service.measurepoint.get"
}

```

## 响应数据格式<response>

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

## 参数说明<parameters>

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
    <td>要获取值的测点的标识符，在此示例中为测点<strong>power</strong>的标识符。</td>
  </tr>
  <tr>
    <td>temp</td>
    <td>String</td>
    <td>可选 </td>
    <td>要获取值的测点的标识符，在此示例中为测点<strong>temp</strong>的标识符。</td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需</td>
    <td>200或设备端定义的错误码，200代表请求成功执行。</td>
  </tr>
  <tr>
    <td>data</td>
    <td>JSON</td>
    <td>可选 </td>
    <td>返回的详细信息。JSON格式 </td>
  </tr>
</table>
