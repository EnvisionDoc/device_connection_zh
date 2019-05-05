# 云端下发测点设置与服务调用（非DTLS加密）

应用可以通过EnOS的API，对通过CoAP接入的设备设置测点或调用服务。其流程如下图：

.. image:: ../../../media/coap_downstream_flow_non_dtls.png 

关于测点设置、服务调用的请求数据格式、相应数据格式、参数说明，参见[向设备发送指令（透传）](../mqtt/downstream/devices/invoke_services_pass)。

当应用对设备设置测点或调用服务后，EnOS Cloud会缓存这个请求，待设备发送上行数据时，在响应消息内通知设备，即EnOS会在响应中包含一个EnOS定义的Option（2100）。其格式如下：

```json
Code: CoAP返回码
Payload: {Payload}
Customized Option 2100: /topic/sys/${ProductKey}/${DeviceKey}/thing/model/down_raw
``` 
其中：

.. csv-table::
    :widths: auto

    "参数", "说明"
    "ProductKey", "设备鉴权密钥"
    "DeviceKey", "设备鉴权密钥"
    "Payload", "EnOS响应请求的payload"

设备在收到这个Option后，向EnOS发送请求以获取命令内容：
```json
GET /topic/sys/${ProductKey}/${DeviceKey}/thing/model/down_raw
Customized Option 2101: ${Sign}
```

`Sign`是用于设备鉴权的数字签名，按照下列方法生成：

1. 按照下列格式，拼接以下字段：

 ` token${Token}sequence${Sequence}`
 其中token为设备鉴权成功后被分配的token，sequence是一个为正的int类型数据，表示session中消息的顺序，第一条消息的sequence值为1，sequence值必须大于最近一次向云端发送的消息的sequence值。如果客户端丢失了最后一条消息的Sequence，则必须重新执行设备鉴权上线过程。<!--丢失最后一条消息的sequence，是指期待的sequence与ACK里包含的sequence不符吗？-->

2. 生成SignKey，方法是取`SHA_256 ( ${DeviceSecret} )`当中第9至24字节。SignKey的长度为16字节。

3. 使用拼接的字段和SignKey计算出Sign，方法是`AES_128(${SignKey}, token${Token}sequence${Sequence})`

4. 将得到的16进制字符串中的字母转换为大写

EnOS的响应格式如下：

```json
Code: CoAP返回码，参考Code说明
Payload: ${Payload}
```

如果有更多的服务调用，EnOS还会在响应中包含Option（2100）。设备也会重复服务调用查询过程。

设备在执行完测点设置或服务调用后，可以将执行的结果按照EnOS设备协议规范定义的响应Topic和响应数据格式，发送给EnOS Cloud。