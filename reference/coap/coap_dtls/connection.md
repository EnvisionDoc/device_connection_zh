# CoAP连接通信

EnOS支持CoAP协议连接通信。CoAP协议适用在资源受限的低功耗设备上，尤其是NB-IoT的设备使用。本文介绍基于CoAP协议、使用DTLS的自主接入流程。

## 基础流程

基于CoAP协议将NB-IoT设备接入EnOS的流程如下图所示：

.. image:: ../../../media/coap_connection_process.png

基础流程说明如下：

1. 用户在EnOS创建产品、关联模型，获取产品的认证密钥productKey、productSecret。

2. 用户应在EnOS注册设备。对于采用一机一密方式认证的用户，需要获得设备认证密钥deviceSecret。
   
3. 设备开发者在NB-IoT设备上集成认证密钥。采用一机一密认证方式的，需要集成produceKey、deviceKey和deviceSecret；采用一型一密认证方式的需要productKey和deviceKey。
   
4. 设备上电入网。

5. 设备发起连接请求，发起DTLS握手请求。

6. EnOS确认请求，完成握手，建立加密的安全通道。

7. （可选）采用一型一密认证方式的设备，此时设备会向EnOS发送查询请求，获取认证密钥deviceSecret。

8. （可选）EnOS将deviceSecret包含在查询请求的响应中发送给设备。

9. 设备使用productKey、productSecre和deviceSecret发起鉴权认证。

10. 鉴权通过，设备激活上线。

11. EnOS发送CoAP协议ACK消息。

## 开始前准备

根据选择的设备安全认证方式，你需要在EnOS Cloud完成相关配置并获取以下认证所需秘钥信息：
- 如果选择一机一密的认证方式，则需逐一注册设备并获取设备的`productKey`，`deviceKey`和`deviceSecret`。有关如何注册设备，参见[注册设备](../../howto/device/manage/creating_device)。
- 如果选择一型一密的认证方式，则需创建设备所属的产品，并获取产品的`productKey`及`productSecret`。有关如何创建产品，参见[创建产品](../../howto/device/manage/creating_device)。通过一型一密认证的设备需在云端从产品配置开启**动态激活**，参见[管理产品](../../howto/device/manage/managing_products)。

有关设备安全认证方式的详细信息，参见[安全认证机制](../../learn/deviceconnection_authentication)。

## 连接CoAP服务器

CoAP服务器地址为`coap-<hostname>`，其中 _hostname_ 是EnOS环境所在的服务器地址，例如EnOS所在环境的服务器地址为*abc.def.com*，则CoAP的服务器地址为*coap-abc.def.com*。

## 建立DTLS安全通道

在建立DTLS安全通道时，必须使用Pre-shared Key（PSK）作为密钥交换算法。EnOS CoAP服务器支持的Cipher Suite有：

- TLS-PSK-WITH-AES-128-CCM-8
- TLS-PSK-WITH-AES-128-CBC-SHA256

## 一机一密接入
  
在DTLS握手的过程中，设备使用的PSK为：

```json
identity:{ProductKey},{DeviceKey},{SecureMode},{Lifetime}
key:SHA-256({DeviceSecret})中第9到第24字节（共16字节）
```

.. csv-table::
   :widths: auto

   "参数", "说明"
   "ProductKey", "设备认证密钥ProductKey"
   "DeviceKey",	"设备认证密钥deviceKey。保证OU下唯一，建议使用NB-IoT模组的IMEI。"
   "SecureMode", "采用“一机一密”认证模式时，SecureMode为2。"
   "Lifetime", "用于判断设备的在线状态。如果设备在Lifetime内没有与云端发生消息交互，设备会被判断为离线。Lifetime的单位为秒，允许设置30 - 86400内的一个值。"
   "DeviceSecret", "设备认证密钥DeviceSecret。"

PSK参数示例：
```
Identity: MuGsF6W4,15bBZlRknc,2,3600
Key:      0x01 0x24 0xDD 0xA7 0xA5 0xC6 0x3F 0x92 0xD9 0x8C 0x2F 0x80 0x9B 0x1E 0x3C 0x36
```

## 一型一密接入

在DTLS握手的过程中，设备使用的PSK为：

```json
identity: {ProductKey},{DeviceKey},{SecureMode},{Lifetime}
key: SHA-256({ProductSecret}) 中第9到第24字节（共16字节）
```

.. csv-table::
   :widths: auto
   
   "参数", "说明"
   "ProductKey", "设备认证密钥ProductKey"
   "DeviceKey", "设备认证密钥deviceKey。保证OU下唯一，建议使用NB-IoT模组的IMEI。"
   "SecureMode", "采用“一型一密”认证模式时，SecureMode为3。"
   "Lifetime", "用于判断设备的在线状态。如果设备在Lifetime内没有与云端发生消息交互，设备会被判断为离线。Lifetime的单位为秒，允许设置30 - 86400内的一个值。"
   "ProductSecret", "产品密钥"

PSK参数示例：
```json
Identity: MuGsF6W4,15bBZlRknc,3,3600
Key:      0x01 0x24 0xDD 0xA7 0xA5 0xC6 0x3F 0x92 0xD9 0x8C 0x2F 0x80 0x9B 0x1E 0x3C 0x36
 
```

完成握手之后，设备应当通过查询请求获取DeviceSecret，请求格式如下：
`GET /topic/ext/session/{productKey}/{deviceKey}/thing/activate/info`

DeviceSecret包含在了该请求的响应中：

```json
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
```

.. csv-table::
   :widths: auto
   
   "参数", "说明"
   "id", "标识消息的唯一ID"
   "assetId", "设备对应的资产ID"
   
获取`deviceSecret`后，设备连接EnOS时，通过设备的`productKey`，`deviceKey`和`deviceSecret`进行认证。




 

 

