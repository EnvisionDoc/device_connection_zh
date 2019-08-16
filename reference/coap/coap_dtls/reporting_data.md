# 设备上报属性、测点、事件

设备可以通过CoAP接入，按照EnOS设备协议规范上报属性信息、测点信息和设备事件。其流程如下图所示：

.. image:: ../../../media/coap_upstream_flow.png

通过CoAP接入的低功耗设备消耗的数据格式常见为二进制数据流，可以使用透传模式上报至EnOS Cloud并在云端通过脚本解析器将数据转成EnOS标准JSON。

当设备通过CoAP协议接入EnOS时，Topic规范和MQTT Topic一致，有关上行消息的请求数据格式、响应数据格式、参数说明，参见[设备上报属性、测点和事件（透传）](../../mqtt/upstream/device_else/report_event_pass)。

响应中除了包括EnOS设备协议规范定义的响应数据（Payload）外，还包括了CoAP返回码，返回码和响应数据的结构如下：

```json
Code: CoAP协议定义的返回码
Payload: {ResponsePayload}
``` 

CoAP协议定义的返回码说明如下：


.. csv-table::
   :widths: auto

   "返回码", "描述", "Payload", "说明"
   "2.04", "Changed", "EnOS支持的响应数据", "正确请求"
   "4.00", "Bad Request", "无", "请求发送的Payload非法"
   "4.01", "Unauthorized", "无", "未授权的请求"
   "4.03", "Forbidden", "无", "禁止的请求"
   "4.04", "Not Found", "无", "请求的路径不存在"
   "4.05", "Method Not Allowed", "无", "请求方法不合法"
   "5.00", "Internal Server Error", "无", "EnOS内部错误"

<!--end-->
