# 上报标签信息

上行
- Request TOPIC: /sys/{productKey}/{deviceKey}/thing/tag/update

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/tag/update_reply

## 请求数据格式

```
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "tagKey": "Temperature",
 "tagValue": "36.8"
 }
 ],
 "method": "thing.tag.update"
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
    <td>标签信息上报的阐述。Params   元素不能超过200个。 </td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需 </td>
    <td>请求方法 </td>
  </tr>
  <tr>
    <td>tagKey</td>
    <td>String</td>
    <td>必需 </td>
    <td>标签的名字 <br>
      长度不超过100字节 <br>
      仅支持字符集 [a-z,  A-Z, 0-9]和下划线。 <br>
      名字的首字母必需是字母或下划线 </td>
  </tr>
  <tr>
    <td>tagValue</td>
    <td>String</td>
    <td>必需 </td>
    <td>标签的值 </td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需 </td>
    <td>结果返回码，200代表请求成功执行。 </td>
  </tr>
  <tr>
    <td>data </td>
    <td>String </td>
    <td>可选 </td>
    <td>返回的详细信息 。JSON格式 </td>
  </tr>
</table>
