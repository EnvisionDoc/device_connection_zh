# 基于CoAP协议的连接

设备开发者可以通过CoAP/DTLS协议，将设备采集的实时数据上报到EnOS，借助EnOS，实现海量设备的安全连接和数据管理能力。NB-IoT设备接入EnOS的流程如下图：

.. image:: ../media/enos_coap.png

1. 设备端集成IoT SDK, 并烧录设备三元组于设备中；
2. 设备通过运营商提供的蜂窝网络连接到EnOS,运营商负责管理设备产生的流量数据和计费，不储存业务数据；
3. 应用开发者可以通过EnOS的API获取设备数据、修改测点属性、调用设备服务，并将获取的数据呈现在应用中。

## CoAP协议标准

EnOS支持的协议标准为RFC 7252 Constrained Application Protocol，具体参考：[RFC 7252](https://tools.ietf.org/html/rfc7252?spm=a2c4g.11186623.2.12.2e2e3cb88Cjj8L)。

## EnOS CoAP约定

支持可确认请求（`CON`），响应会以`Piggybacked`形式在`ACK`消息中返回。

实现了RFC 7252协议规定的CoAP缓存功能，因此要求设备对于每一个新的请求，使用不同CoAP Message ID和Token。

EnOS支持CoAP协议中的Block-wise传输功能，具体参考：[RFC 7959](https://tools.ietf.org/html/rfc7959)。

## DTLS安全通道

设备请求连接到指定的CoAP服务器，然后建立DTLS安全通道。连接过程主要步骤如下:

1. 设备首先发送DTLS握手请求
2. 设备端和EnOS根据预定义的算法协商PSK参数
3. 设备端发送pskid和len参数到EnOS进行校验，成功后建立安全连接通道。

建立安全通道之后，设备发起身份认证请求，如果采用加密方式，此步骤可与上一步DTLS握手合并；如果采用非加密方式，则采用三元组认证方式，支持选择一机一密和一型一密，如果采用后者需先返回devicesecret再认证。

EnoS使用DTLS v1.2保证通道安全，具体参考：[RFC 6347](https://tools.ietf.org/html/rfc6347?spm=a2c4g.11186623.2.13.2e2e3cb88Cjj8L)。


## 说明与限制

当设备通过CoAP协议接入EnOS时，Topic规范和MQTT Topic一致，CoAP协议内接口对于所有topic和MQTT Topic可以复用。

EnOS CoAP接入目前不支持资源发现功能，不支持资源观察功能。

通过CoAP传输数据的大小依赖于MTU的大小，建议在1KB以内。

## 下一步

参考[CoAP协议规范](/docs/device-connection/zh_CN/dev/reference/coap/index)将设备接入EnOS并复用MQTT topic与EnOS Cloud进行数据传输。
