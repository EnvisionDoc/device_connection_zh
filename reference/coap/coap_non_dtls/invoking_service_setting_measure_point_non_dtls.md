# 云端下发测点设置与服务调用（非DTLS加密）

应用可以通过EnOS的API，对通过CoAP接入的设备设置测点或调用服务。其流程如下图：

.. image:: ../../../media/coap_downstream_flow_non_dtls.png 

关于测点设置、服务调用的请求数据格式、相应数据格式、参数说明，参见[向设备发送命令（透传）](../../mqtt/downstream/invoke_services_pass)和[调用设备服务 (非透传)](../../mqtt/downstream/invoke_services_nopass)。

当应用对设备设置测点或调用服务后，EnOS Cloud会缓存这个请求，待设备发送上行数据时，在响应消息内通知设备，即EnOS会在响应中包含一个EnOS定义的Option（2100），同时将该命令的payload包含在Option 2102中。其格式如下：

```json
Code: CoAP返回码
Payload: {Payload}
Customized Option 2100: /topic/sys/${ProductKey}/${DeviceKey}/thing/model/down_raw
Customized Option 2102: {Down-Raw Payload}
``` 
其中：

.. csv-table::
    :widths: auto

    "参数", "说明"
    "ProductKey", "设备鉴权密钥"
    "DeviceKey", "设备鉴权密钥"
    "Payload", "EnOS响应请求的payload"

设备在执行完测点设置或服务调用后，可以将执行的结果按照EnOS设备协议规范定义的响应Topic和响应数据格式，发送给EnOS Cloud。

如果有更多的命令，EnOS还会在响应中包含Option(2100)和Option(2102)提示处理更多的命令。

