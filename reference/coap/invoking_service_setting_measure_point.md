# 云端下发测点设置与服务调用

应用可以通过EnOS的API，对通过CoAP接入的设备设置测点或调用服务。

关于测点设置、服务调用的请求数据格式、相应数据格式、参数说明，参见[向设备发送指令（透传）](../mqtt/downstream/devices/invoke_services_pass)。

当应用对设备设置测点或调用服务后，EnOS Cloud会缓存这个请求，待设备发送上行数据时，在响应消息内通知设备，即EnOS会在响应中包含一个EnOS定义的Option（2100）。

响应：

```json
Code: CoAP返回码
Payload: {RequestPayload}
Customized Option 2100: /topic/{RequestTopic}
``` 

设备在收到这个Option后，应用可以通过请求调用服务。

查询服务调用请求：`GET /topic/{RequestTopic}`

响应：

```json
Code: CoAP返回码，参考Code说明
Payload: 
```

如果有更多的服务调用，EnOS还会在响应中包含Option（2100）。设备也会重复服务调用查询过程。

设备在执行完测点设置或服务调用后，可以将执行的结果按照EnOS设备协议规范定义的响应Topic和响应数据格式，发送给EnOS Cloud。