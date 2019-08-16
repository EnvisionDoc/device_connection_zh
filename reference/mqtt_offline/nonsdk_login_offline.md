# 使用MQTT协议与EnOS Cloud建立连接

本文参数如何通过MQTT协议建立从第三方系统到EnOS Cloud的连接。

支持的MQTT版本：

- 如果你使用基于证书的双向认证，在端口18883上使用 MQTT v3.1.1。
- 如果你使用基于密钥的单向认证，在端口11883上使用 MQTT v3.1.1。

## 直接使用MQTT协议

您可以直接使用MQTT协议将第三方系统连接到EnOS Cloud。第三方系统的CONNECT数据包中包含以下值：<!--Broker URL放在哪里？什么格式？ -->

```
  mqttClientId: clientId+"|securemode=2,signmethod=sha1,timestamp=132323232|"
  mqttUsername: deviceKey+"&"+productKey
  mqttPassword: uppercase(sign_hmac(<content><deviceSecret>))
```

- **mqttClientId** 部分:

  - _clientId_: 必填项。可指定使用MAC地址或设备序列号。不可超过64个字符。在``||``中的参数为必填项。
  - _securemode_: 必填项。表示所使用的安全模式。当前只支持`securemode=2`。
  - _signmethod_: 必填项。表示所使用的安全模式签名方法。当前只支持`signmethod=sha1`。
  - _timestamp_: 必填项。表示当前的时间毫秒值。
  - 示例：123456|securemode=2,signmethod=hmacsha1,timestamp=1548753362502|。

- **mqttUsername** 部分:

  - 设备_deviceKey_与_productKey_的值，当第三方系统完成预配后，可在EnOS Console上查看该值。
  - 示例：tPbZGCdmaE&KV315idW。

- **mqttPassword** 部分:<!--是否在消息集成通道建立通道后自动获得？-->

 <!-- - **mqttPassword** 可以由[password小工具](_static/enosmqttsign/index.html)生成，传入指定的参数可以自动生产。-->
  - _content_: 为_clientID_, _deviceKey_, _productKey_, _timestamp_,和他们值的串联组合。 将参数按照字母顺序排序，然后将参数和值依次拼接（无拼接符号）。
  - _deviceSecret_: 第三方系统_deviceSecret_的值，当设备完成预配后，可在EnOS Console上查看该值。该值紧跟_content_之后，无需空着和符号。
  - 示例：E42C1186C3BFFE98E1EA8AB320CD800D579212AA。

    下面的列子为当_clientId_=`123456`, _deviceKey_=`tPbZGCdmaE`, _productKey_=`KV315idW`, _timestamp_=`1548753362502`, _deviceSecret_=`M3yy874uGY8INjEGddTR`， _mqttPassword_ 的形式应该如下。

    ```
    sign=uppercase(sha1(clientId123456deviceKeytPbZGCdmaEproductKeyKV315idWtimestamp1548753362502M3yy874uGY8INjEGddTR))
    ```

.. note:: `timestamp`的值必须和**mqttClientId**部分的`timestamp`保持一致。

<!--end-->
