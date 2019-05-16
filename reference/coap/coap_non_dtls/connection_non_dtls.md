# CoAP连接通信（非DTLS加密）

EnOS支持CoAP协议连接通信。CoAP协议适用在资源受限的低功耗设备上，尤其是NB-IoT的设备使用。本文介绍基于CoAP协议、不使用DTLS的接入流程。

基于CoAP的非DTLS加密连接，所使用的CoAP协议标准和限制，与DTLS加密连接相同，参见[基于CoAP协议的连接](../../../learn/enos_coap)。

## 连接安全

设备需要具备SHA-256和AES-128算法能力，以生成鉴权时的数字签名。

## 基础流程

基于CoAP协议将NB-IoT设备接入EnOS的流程如下图所示：

.. image:: ../../../media/coap_connection_process_non_dtls.png

基础流程说明如下：

1. 用户在EnOS创建产品、关联模型，获取产品的认证密钥productKey、productSecret。

2. 用户应在EnOS注册设备，需要获得设备认证密钥deviceSecret。
   
3. 设备开发者在NB-IoT设备上集成认证密钥：productKey、deviceKey和deviceSecret。
   
4. 设备上电入网。

5. 设备向EnOS发送鉴权请求，请求中包含了认证密钥productKey、deviceKey、deviceSecret以及数字签名。

6. 鉴权通过，设备激活上线。

7. EnOS发送CoAP协议ACK消息。

## 开始前准备

- 逐一注册设备并获取设备的`productKey`，`deviceKey`和`deviceSecret`。有关如何注册设备，参见[注册设备](../../howto/device/manage/creating_device)。
- 确保设备具有AES-128和SHA-256加密能力。

## 连接CoAP服务器

CoAP服务器地址为`coap-<hostname>`，其中 _hostname_ 是EnOS环境所在的服务器地址，例如EnOS所在环境的服务器地址为*abc.def.com*，则CoAP的服务器地址为*coap-abc.def.com*。

## 设备鉴权并连接
  
1. 设备发送鉴权请求，其格式为：
  ```json
   POST /auth/${ProductKey}/${DeviceKey}
   Payload: { "secureMode": ${SecureMode}, "lifetime": ${Lifetime}, "sign": ${sign} }
  ```

  其中：

  .. csv-table::
    :widths: auto

    "参数", "说明"
    "ProductKey", "设备鉴权密钥"
    "DeviceKey", "设备鉴权密钥，建议使用设备的IMEI号作为deviceKey,保证OU下唯一"
    "SecureMode", "认证模式，此种认证方式下为整数2"
    "Lifetime", "用于判断设备的在线状态。如果设备在Lifetime内没有与云端发生消息交互，设备会被判断为离线。Lifetime的单位为秒，允许设置30 - 86400内的一个整数值"
    "Sign", "用于验证设备身份的数字签名"
    

 数字签名按照如下方法生成：

   1. 按照下列格式，拼接请求中的字段：
      `deviceKey${DeviceKey}lifetime${Lifetime}productKey${ProductKey}secureMode${secureMode}`

    2. 在该拼接字段结尾附上${DeviceSecret}，生成的字段如下：
     `deviceKey${DeviceKey}lifetime${Lifetime}productKey${ProductKey}secureMode${secureMode}${DeviceSecret}`

    3. 使用SHA-256算法获得该拼接字段的摘要，并将其中的字母转换为大写。

2. 鉴权通过，EnOS在鉴权请求的相应中包含CoAP协议定义的返回码和一个token，用于后续session的鉴权。响应的格式如下：
   ```json
   Code: CoAP返回码，参考CoAP协议说明
   Payload: 如果设备鉴权上线成功 { "token" : ${Token} }
   ```

   Token是EnOS随机分配的一个字符串，用于为后续消息鉴权。Token具有一定的有效时间，与设备鉴权时请求的Lifetime相关。如果设备在Lifetime内不与云端通信，Token过期。当Token过期后，设备必须重新执行鉴权流程，以获得新的Token。
