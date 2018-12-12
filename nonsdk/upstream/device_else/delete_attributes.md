# 删除属性

设备端删除设备属性的请求，云端根据上报的请求，删除对应的属性值。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/delete`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/delete_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": [
   "attributes": ["attr1", "attr2", "attr3"]
 ],
 "method": "thing.attribute.delete"
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
    <th>参数</th>
    <th>类型</th>
    <th>是否必需</th>
    <th>描述</th>
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
    <td>删除属性所需的参数 </td>
  </tr>
  <tr>
    <td>attributes</td>
    <td>Array</td>
    <td>可选 </td>
    <td>需要删除的属性的标识符的列表。一次最多可删除200个属性。如果为空，则不会删除任何属性。</td>
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
    <td>结果返回码，200代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>JSON</td>
    <td>可选</td>
    <td>返回的详细信息。JSON格式 </td>
  </tr>
</table>
