# 标签信息删除

- Request TOPIC /sys/{productKey}/{deviceName}/thing/tag/delete

- Reply TOPIC /sys/{productKey}/{deviceName}/thing/tag/delete_reply

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {
    "tags": ["tag1", "tag2"]
 },
 "method": "thing.tag.delete"
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
    <td>List</td>
    <td>必需 </td>
    <td>删除标签信息的参数 </td>
  </tr>
  <tr>
    <td>tags</td>
    <td>List</td>
    <td>必需 </td>
    <td>标签标识符。如为空值，则不做任何操作。 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
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
