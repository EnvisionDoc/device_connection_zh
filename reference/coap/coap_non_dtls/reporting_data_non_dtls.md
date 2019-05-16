# 设备上报属性、测点、事件（非DTLS加密）

设备可以通过CoAP接入，按照EnOS设备协议规范上报属性信息、测点信息和设备事件。其流程如下图所示：

.. image:: ../../../media/coap_upstream_flow_non_dtls.png 

通过CoAP接入的低功耗设备消耗的数据格式常见为二进制数据流，可以使用透传模式上报至EnOS并在云端通过脚本解析器将数据转成EnOS标准JSON。

当设备通过CoAP协议接入EnOS时，Topic规范和MQTT Topic一致，有关上行消息的请求数据格式、响应数据格式、参数说明，参见[设备上报属性、测点和事件（透传）](../mqtt/upstream/device_else/report_event_pass)

设备上报数据的格式如下：

```json
POST /topic/sys/${ProductKey}/${DeviceKey}/thing/model/up_raw
Payload: ${Payload}
Customized Option 2101 : ${Sign}
```

其中：

.. csv-table::
    :widths: auto

    "参数", "说明"
    "ProductKey", "设备鉴权密钥"
    "DeviceKey", "设备鉴权密钥"
    "Payload", "设备需要上报的数据"
    "Sign", "用于验证设备身份的数字签名"
    
数字签名按照如下方法生成：

1. 按照下列格式，拼接以下字段：

 ` token${Token}sequence${Sequence}`
 其中token为设备鉴权成功后被分配的token，sequence是一个为正的int类型数据，表示session中消息的顺序，第一条消息的sequence值为1，sequence值必须大于最近一次向云端发送的消息的sequence值。如果客户端丢失了最后一条消息的Sequence，则必须重新执行设备鉴权上线过程。<!--丢失最后一条消息的sequence，是指期待的sequence与ACK里包含的sequence不符吗？-->

2. 生成SignKey，方法是取`SHA_256 (${DeviceSecret})`当中第9至24字节。SignKey的长度为16字节。

3. 使用拼接的字段和SignKey计算出Sign，方法是`AES_128(${SignKey}, token${Token}sequence${Sequence})`,模式是CBC。

4. 将得到的16进制字符串中的字母转换为大写


响应中除了包括EnOS设备协议规范定义的响应数据（Payload）外，还包括了CoAP返回码，响应的格式如下：

```json
Code: CoAP协议定义的返回码
Payload: {Payload}
``` 


