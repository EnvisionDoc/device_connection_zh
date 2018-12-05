# 获取标签信息

上行
- Request TOPIC: /sys/{productKey}/{deviceKey}/thing/tag/query

- Reply TOPIC: /sys/{productKey}/{deviceKey}/thing/tag/query_reply

## 请求数据格式


```
{
 "id": "123",
 "version": "1.0",
 "params": {
   "tags": ["tag1", "tag2"]
 },
 "method": "thing.tag.query"
}

```

## 响应数据格式

```
{
 "id": "123",
 "code": 200,
 "data": {
   "tag1": "value1",
   "tag2": "value2"
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
    <td>可选</td>
    <td>消息ID号，保留值</td>
  </tr>
  <tr>
    <td>version</td>
    <td>String</td>
    <td>必需</td>
    <td>协议版本号，目前协议版本1.0</td>
  </tr>
  <tr>
    <td>params</td>
    <td>Object</td>
    <td>必需</td>
    <td>标签信息上报的阐述。Params   元素不能超过200个。</td>
  </tr>
  <tr>
    <td>tags</td>
    <td>List</td>
    <td>必需</td>
    <td>请求的tags列表，当tags为空时，表示请求设备所有的tag</td>
  </tr>
  <tr>
    <td>method</td>
    <td>String</td>
    <td>必需</td>
    <td>请求方法</td>
  </tr>
  <tr>
    <td>code</td>
    <td>Integer</td>
    <td>必需</td>
    <td>结果返回码，200代表请求成功执行。</td>
  </tr>
</table>
