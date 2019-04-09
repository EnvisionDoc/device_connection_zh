# CoAP连接通信

物联网平台支持CoAP协议连接通信。CoAP协议适用在资源受限的低功耗设备上，尤其是NB-IoT的设备使用。本文介绍基于CoAP协议进行设备接入的流程，及使用DTLS的自主接入流程。

## 基础流程
基于CoAP协议将NB-IoT设备接入物联网平台的流程如下图所示：

.. image:: ../../media/coap_connection_process.png

基础流程说明如下：

1. 开发者在EnOS控制台创建产品、关联模型。

2. 开发者进行设备端开发，初始化SDK，通过AT指令读取模组IMEI号并设置CoAP服务器地址。
   
3. 开发者将设备IMEI号作为devicekey预注册到EnOS，获得设备三元组（ProductKey，DeviceKey和DeviceSecret）。
   
4. 上线设备，通过运营商的蜂窝网络进行入网。需要联系当地运营商，确保设备所属地区已经覆盖NB-IoT网络，并已具备NB-IoT入网能力。

## 连接CoAP服务器

服务器地址为： xxxx.xxx.xxx.xxxx，端口为5684。

## 建立DTLS安全通道

在建立DTLS安全通道时，必须使用Pre-shared Key（PSK）作为密钥交换算法。EnOS CoAP服务器支持的Cipher Suite有：

- TLS-PSK-WITH-AES-128-CCM-8
- TLS-PSK-WITH-AES-128-CBC-SHA256
  
在DTLS握手的过程中，设备使用的PSK为：

identity:`{ProductKey},{DeviceKey},{SecureMode},{Lifetime}`

key:`SHA-256({DeviceSecret})`中第9到第24字节（共16字节）

.. csv-table::
   :widths: auto

   "参数", "说明"
   "ProductKey", "设备三元组中的ProductKey"
   "DeviceKey",	"设备三元组中的deviceKey。保证OU下唯一，建议使用NB-IoT模组的IMEI。"
   "SecureMode", "采用“一机一密”认证模式时，SecureMode固定为2。"
   "Lifetime", "用于判断设备的在线状态。如果设备在Lifetime内没有与云端发生消息交互，设备会被判断为离线。Lifetime的单位为秒，允许设置30 - 86400内的一个值。"
   "DeviceSecret", "设备三元组中的DeviceSecret。"

3.设备上报属性、测点、事件

设备可以通过CoAP接入，按照EnOS设备协议规范上报属性信息、测点信息和设备事件。

对于支持透传模式的设备，可以使用透传模式，上报原始数据如二进制数据流。

 

上行数据请求：

POST /topic/{RequestTopic}
Payload: {RequestPayload}
RequestTopic	
EnOS设备协议规范中定义的请求Topic。

例如上报属性信息使用的RequestTopic为：/sys/{productKey}/{deviceKey}/thing/attribute/update

RequestPayload	EnOS设备协议规范中定义的请求数据格式。
 

响应：

Code: CoAP返回码，参考Code说明
Payload: {ResponsePayload}
 

返回码说明：

2.04	Changed	设备支持的响应数据如二进制数据	正确请求
4.00	Bad Request	无	请求发送的Payload非法
4.01	Unauthorized	无	未授权的请求
4.03	Forbidden	无	禁止的请求
4.04	Not Found	无	请求的路径不存在
4.05	Method Not Allowed	无	请求方法不合法
5.00	Internal Server Error	无	EnOS Cloud内部错误
ResponsePayload	
EnOS设备协议规范中定义的响应数据格式

 

 

1. 设备测点设置与服务调用

应用可以通过EnOS开放的API，对CoAP接入的设备设置测点或调用服务。

对于支持透传模式的设备，可以使用透传模式，下发原始数据如二进制数据流。

当应用对设备设置测点或调用服务后，EnOS Cloud会缓存这个二进制流，待设备发送上行数据时，在响应消息内通知设备。设备会在响应中一个平台定义的Option（2100）。

响应：

Code: 上行数据返回码
Payload: 上行数据响应Payload
Customized Option 2100: /topic/{RequestTopic}
 


设备在收到这个Option后，可以通过服务调用请求，获取服务调用。

查询服务调用请求：

GET /topic/{RequestTopic}

响应：

Code: CoAP返回码，参考Code说明
Payload: {RequestPayload}

如果有更多的服务调用

 

Code: CoAP返回码，参考Code说明
Payload: {RequestPayload}
Customized Option 2100: /topic/{RequestTopic}
 

如果有多个服务调用，设备可以重复这个服务调用查询过程。

 

设备在执行完测点设置或服务调用后，可以将执行的结果按照EnOS设备协议规范定义的响应Topic和响应数据格式，发送给EnOS Cloud。响应的发送方式是：

POST /topic/{ResponseTopic}
Payload: {ResponsePayload}
 

 

RequestTopic	
EnOS设备协议规范中定义的请求Topic。

例如设置测点的RequestTopic为：/sys/{productKey}/{deviceKey}/thing/service/measurepoint/set

RequestPayload	EnOS设备协议规范中定义的请求数据格式。
ResponseTopic	
EnOS设备协议规范中定义的响应Topic

例如设置测点的ResponseTopic为：/sys/{productKey}/{deviceKey}/thing/service/measurepoint/set_reply

ResponsePayload	EnOS设备协议规范中定义的响应数据格式。
 

4. 通过“一型一密”方式接入设备
1. 连接CoAP服务器，服务器地址为： xxxx.xxx.xxx.xxxx（根据联调需要提供），端口为5684.

2. 在建立DTLS安全通道时，必须使用Pre-shared Key（PSK）作为密钥交换算法。EnOS CoAP服务器支持的Cipher Suite有：

TLS-PSK-WITH-AES-128-CCM-8
TLS-PSK-WITH-AES-128-CBC-SHA256
在DTLS握手的过程中，设备使用的PSK为：

identity: {ProductKey},{DeviceKey},{SecureMode},{Lifetime}

key: SHA-256({ProductSecret}) 中第9到第24字节（共16字节）

ProductKey	设备三元组中的ProductKey
DeviceKey	设备三元组中的deviceKey。保证OU下唯一，建议使用NB-IoT模组的IMEI
SecureMode	采用”一型一密“认证模式时，SecureMode固定为3
Lifetime	用于判断设备的在线状态。如果设备在Lifetime内没有与云端发生消息交互，设备会被判断为离线。Lifetime的单位为秒，允许设置30 - 86400内的一个值
ProductSecret	产品密钥

3.获取设备密钥

采用“一型一密”方式的设备，在建立DTLS安全通道后，应当首先获取自己的设备密钥。

设备密钥查询请求：

GET /topic/ext/session/{productKey}/{deviceKey}/thing/activate/info
 

响应：

Code: CoAP返回码，参考Code说明
Payload: {
    "id": {id},
    "version": "1.0",
    "method": "thing.activate.info",
    "params":{
        "assetId": {assetId},
        "productKey": {productKey},
        "deviceKey": {deviceKey},
        "deviceSecret": {deviceSecret}
    }
}
productKey	
设备三元组中的ProductKey

deviceKey	
设备三元组中的DeviceKey

deviceSecret	设备三元组中的DeviceSecret
id	标识消息的唯一ID
assetId	设备对应的资产ID
 

设备在获取到设备密钥后，应当持久化保存这个这个设备密钥。

设备应当通过“一机一密”方式重新连接EnOS Cloud，上报设备的属性、测点、数据，或处理设备的测点设置与服务调用。