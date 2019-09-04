# 设备上报属性、测点、事件（非DTLS加密）

设备可以通过CoAP接入，按照EnOS设备协议规范上报属性信息、测点信息和设备事件。其流程如下图所示：

.. image:: ../../../media/coap_upstream_flow_non_dtls.png 

通过CoAP接入的低功耗设备消耗的数据格式常见为二进制数据流，可以使用透传模式上报至EnOS并在云端通过脚本解析器将数据转成EnOS标准JSON。

当设备通过CoAP协议接入EnOS时，Topic规范和MQTT Topic一致，有关上行消息的请求数据格式、响应数据格式、参数说明，参见[设备上报属性、测点和事件（透传）](../../mqtt/upstream/device_else/report_event_pass)和[设备事件上报](../../mqtt/upstream/device_else/report_event_nopass)。

设备上报数据的格式（透传）如下：

```json
POST /topic/sys/${ProductKey}/${DeviceKey}/thing/model/up_raw
Payload: ${Payload}
```

其中：

.. csv-table::
    :widths: auto

    "参数", "说明"
    "ProductKey", "设备鉴权密钥"
    "DeviceKey", "设备鉴权密钥"
    "Payload", "设备需要上报的数据"


响应中除了包括EnOS设备协议规范定义的响应数据（Payload）外，还包括了CoAP返回码，响应的格式如下：

```json
Code: CoAP协议定义的返回码
Payload: {Payload}
``` 


