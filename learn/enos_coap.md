# 基于CoAP协议的连接

设备开发者可以通过CoAP/DTLS协议，将设备采集的实时数据上报到EnOS Cloud，借助EnOS，实现海量设备的安全连接和数据管理能力。

设备请求连接到指定的CoAP服务器，开发者可选加密和非加密端口。如果选择加密端口，则设备首先发送DTLS握手请求，设备端和云端根据预定义的算法协商PSK参数，设备端发送pskid和len参数到云端进行校验，成功后建立安全连接通道。

建立安全通道之后，设备发起身份认证请求，如果采用加密方式，此步骤可与上一步DTLS握手合并；如果采用非加密方式，则采用三元组认证方式，支持选择一机一密和一型一密，如果采用后者需先返回devicesecret再认证。



## 协议版本

支持RFC 7252 Constrained Application Protocol协议，具体请参考：[RFC 7252](https://tools.ietf.org/html/rfc7252?spm=a2c4g.11186623.2.12.2e2e3cb88Cjj8L)。

## 通道安全

使用DTLS v1.2保证通道安全，具体请参考：[RFC 6347](https://tools.ietf.org/html/rfc6347?spm=a2c4g.11186623.2.13.2e2e3cb88Cjj8L)。

## EnOS CoAP约定

EnOS CoAP接入支持可确认请求（CON），响应会以Piggybacked形式在ACK消息中返回。

EnOS CoAP接入实现了RFC 7252协议规定的CoAP缓存功能，因此要求设备对于每一个新的请求，使用不同CoAP Message ID和Token。

EnOS支持CoAP协议中的Block-wise传输功能，具体参考：[RFC 7959](https://tools.ietf.org/html/rfc7959)。

## 说明与限制

Topic规范和MQTT Topic一致，CoAP协议内接口对于所有topic和MQTT Topic可以复用。

EnOS CoAP接入目前不支持资源发现功能，不支持资源观察功能。

通过CoAP传输数据的大小依赖于MTU的大小，建议在1KB以内。


