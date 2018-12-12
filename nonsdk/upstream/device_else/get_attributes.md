# 获取属性信息

设备端从云端获取属性，云端根据需要查询的属性ID，返回对应的属性内容。当需要查询的属性内容为空时，云端返回设备的所有属性。

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/query`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/attribute/query_reply`

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": {
   "attributes": ["attr1", "attr2", "attr3"]
 },
 "method": "thing.attribute.query"
}
```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "data": {
   "attr1": {
       "value": 1.0,
       "value2": 9
     },
   "attr2": 1.02,
   "attr3": [1.02, 2.02, 7.93]
 }
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
    <td>查询属性所需的参数</td>
  </tr>
  <tr>
    <td>attributes</td>
    <td>Array</td>
    <td>可选 </td>
    <td>需要查询的属性的标识符的列表。一次最多可查询200个属性。如果为空，云端会将该设备的所有属性返回。 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>attr1</td>
    <td>String</td>
    <td>必需 </td>
    <td>查询的属性的标识符 </td>
  </tr>
  <tr>
    <td>value</td>
    <td>Struct</td>
    <td>Integer</td>
    <td>返回的该属性的详细信息。在本例中，该属性的数据类型为struct，所以为<strong>value</strong>与它的值被返回。</td>
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
