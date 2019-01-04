# 上报属性信息

设备端上报属性的新值，云端根据上报的请求，更新对应的属性内容。

上行
- 请求TOPIC：`/sys/{productKey}/{deviceKey}/thing/attribute/update`

- 响应TOPIC：`/sys/{productKey}/{deviceKey}/thing/attribute/update_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {
 "attributes": {
 "attr1": {
     "value": 1.0,
     "value2": 9
   },
 "attr2": 1.02,
 "attr3": [1.02, 2.02, 7.93]
 }
 },
 "method": "thing.attribute.update"
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
    <td>上报属性所需的参数</td>
  </tr>
  <tr>
    <td>attributes</td>
    <td>Array</td>
    <td>可选</td>
    <td>需上报的属性的标识符的列表。一次最多可上报200个属性。如果为空，则无上报的属性。</td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>attr1</td>
    <td>struct</td>
    <td>必需 </td>
    <td>要上报的属性的标识符，在此示例中为属性<strong>attr1</strong>的标识符。此处设置的格式必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为struct时，此处的格式必须与模型中保持一致，为<strong>value</strong>和<strong>value2</strong>。</td>
  </tr>
  <tr>
    <td>value</td>
    <td>String</td>
    <td>必需 </td>
    <td>该属性中的参数名。在本例中，为<strong>value</strong>。此处设置的值必须与服务的数据类型匹配。例如，当此参数的数据类型在模型中设置为integer时，那么值必须为integer。</td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需</td>
    <td>结果返回码，200代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data</td>
    <td>JSON</td>
    <td>可选 </td>
    <td>返回的详细信息。JSON格式 </td>
  </tr>
</table>
