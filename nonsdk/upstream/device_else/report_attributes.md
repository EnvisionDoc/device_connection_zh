# 属性上报

设备端上报属性的新值，云端根据上报的请求，更新对应的属性内容。

- Request TOPIC：/sys/{productKey}/{deviceKey}/thing/attribute/update

- Reply TOPIC：/sys/{productKey}/{deviceKey}/thing/attribute/update_reply

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": [
 "attributes": {
 "attr1": {
     "value": 1.0,
     "value2": "9"
   },
 "attr2": 1.02,
 "attr3": [1.02, 2.02, 7.93]
 }
 ],
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
    <td>请求参数 </td>
  </tr>
  <tr>
    <td>attributes</td>
    <td>Array</td>
    <td>可选 </td>
    <td>属性的标识符，用于设置具体的属性键值对 </td>
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
    <td>属性的标识符 </td>
  </tr>
  <tr>
    <td>value</td>
    <td>String</td>
    <td>必需 </td>
    <td>标签的值 </td>
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
