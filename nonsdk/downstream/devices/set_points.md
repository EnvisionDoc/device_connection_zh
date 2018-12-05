# 设置测点

设备接受到设置设备测点请求信息后会更新设备测点信息。

**注意**：根据物模型中的输入参数和输出参数来配置下列的参数。

下行
- TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/set

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/set_reply

## 请求数据格式

```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"temperature": 30.5
	},
	"method": "thing.service.measurepoint.set"
}

```

## 响应数据格式

```
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
    <td>设置设备测点的参数 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>temperature</td>
    <td>String</td>
    <td>可选 </td>
    <td>属性之一 </td>
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
