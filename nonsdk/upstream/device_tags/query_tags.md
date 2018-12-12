# 获取标签信息

上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/tag/query`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/tag/query_reply`

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
    <th>参数</th>
    <th>类型</th>
    <th>是否必需</th>
    <th>描述</th>
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
    <td>查询标签信息所需的参数。一次最多可查询200个标签。</td>
  </tr>
  <tr>
    <td>tags</td>
    <td>List</td>
    <td>必需</td>
    <td>查询的标签列表。当值为空时，将返回所有的设备标签。</td>
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
