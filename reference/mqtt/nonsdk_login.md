# 与EnOS Cloud建立连接

本文参数如何通过MQTT协议建立从设备到EnOS Cloud的连接。

支持的MQTT版本：

- 如果你使用基于证书的双向认证，在端口18883上使用 MQTT v3.1.1。
- 如果你使用基于密钥的单向认证，在端口11883上使用 MQTT v3.1.1。

## 直接使用MQTT协议

您可以直接使用MQTT协议将设备连接到EnOS Cloud。设备的CONNECT数据包中包含以下值：

```java
  mqttClientId: {clientId}|securemode={secureMode},signmethod=sha256,timestamp={timeStamp}|
  mqttUsername: {deviceKey}&{productKey}
  mqttPassword: toUpperCase(sha256(content{deviceSecret}/{productSecret}))
```

- **mqttClientId** 部分:

  - `clientId`: 变量，必填项。可指定使用MAC地址或设备序列号。不可超过64个字符。

  在``||``中的参数为必填项。

  - `securemode`: 必填项。表示所使用的安全模式。
   - 使用静态激活认证方式的设备，`securemode=2`
   - 使用动态激活认证方式的设备，`securemode=3`
  - `signmethod`: 必填项，当前支持sha256。表示使用SHA256签名算法。
  - `timestamp`: 变量，必填项。表示当前的时间的UNIX时间戳毫秒值。


- **mqttUsername** 部分:

  - `deviceKey` ：变量，设备的device key，可以在EnOS控制台上获取。
  - `productKey`：变量，设备的product key，可以在EnOS控制台上获取。

  例如，某设备的`deviceKey`为`abcdefg`，`productKey`为`1234567`。则此处的`mqttUsername`为`abcdefg&1234567`

- **mqttPassword** 部分:

  <!--- **mqttPassword** 可以由[password小工具](../../`static/nonsdk`enosmqttsign`index.html)生成，传入指定的参数可以自动生成。-->

 使用静态激活和动态激活认证方式的设备，采用不同的方式生成`mqttPassword`

### 静态激活

静态激活认证过程中，`productKey`、`deviceKey`、和 `deviceSecret` 都已经在设备尝试认证并登陆EnOS之前，配置在了设备端。在 **设备管理 > 设备管理**中创建设备之后，你就可以获取`productKey`、`deviceKey`、和 `deviceSecret`。

对于静态激活认证方式：
```java
mqttPassword: toUpperCase(sha256(content{deviceSecret}))
```

将`content`和`deviceSecret`字段拼接以生成`mqttPassword`。各字段含义如下：

- `content`: 为 `clientID` , `deviceKey`, `productKey`, `timestamp`,和他们值的串联组合。 将参数按照字母顺序排序，然后将参数和值依次拼接（无字符或空格）。
- `deviceSecret`: 设备 `deviceSecret` 的值，当设备完成创建后，可在EnOS Console上查看该值。该值紧跟 `content` 之后，无需空格和符号。

.. note:: `timestamp`的值必须和 **mqttClientId** 部分的 `timestamp` 保持一致。

举例说明，当参数如下设定：
- `clientId`=`123456`
- `deviceKey`=`tPbZGCdmaE`
- `productKey`=`KV315idW`
- `timestamp`=`1548753362502`
- `deviceSecret`=`M3yy874uGY8INjEGddTR`

此时，`mqttClientId`为：

```java
123456|securemode=2,signmethod=sha256,timestamp=1548753362502|
```

`mqttUsername`为：

```java
tPbZGCdmaE&KV315idW
```

`mqttPassword`为：

```java
mqttPassword = toUpperCase(sha256(clientId123456deviceKeytPbZGCdmaEproductKeyKV315idWtimestamp1548753362502M3yy874uGY8INjEGddTR))
```

### 动态激活

要使用动态激活认证方式，你需要在 **设备管理 > 产品管理 > 产品详情** 中， 打开 **动态激活** 开关。

对于动态激活认证方式：
```java
mqttPassword: toUpperCase(sha256({content}{productSecret}))
```

将`content`和`productSecret`字段拼接以生成`mqttPassword`。各字段含义如下：

- `content`: 为 `clientID` , `deviceKey`, `productKey`, `timestamp`,和他们值的串联组合。 将参数按照字母顺序排序，然后将参数和值依次拼接（无字符或空格）。
- `productSecret`: 设备 `productSecret` 的值，当设备完成创建后，可在EnOS Console上查看该值。该值紧跟 `content` 之后，无需空格和符号。

.. note:: `timestamp`的值必须和 **mqttClientId** 部分的 `timestamp` 保持一致。

举例说明，当参数如下设定：
- `clientId`=`123456`
- `deviceKey`=`tPbZGCdmaE`
- `productKey`=`KV315idW`
- `timestamp`=`1548753362502`
- `productSecret`=`M3yy874uGY8INjEGddTR`

此时，`mqttPassword`为：

```java
mqttPassword = toUpperCase(sha256(clientId123456deviceKeytPbZGCdmaEproductKeyKV315idWtimestamp1548753362502M3yy874uGY8INjEGddTR))
```

在动态激活认证过程中，`productKey`、`productSecret`、和`deviceKey`已经预先配置在了设备当中。当设备尝试认证并登陆EnOS时，设备会发送带有`productKey`、`productSecret`、和`deviceKey`的请求以获取`deviceSecret`。如果设备通过认证，设备需要订阅以下topic来获得`deviceSecret`：

```
/ext/session/{productKey}/{deviceKey}/thing/activate/info
```

EnOS会在发送给设备的响应中，包含`deviceSecret`，举例如下：

```json
{
    "id": "1",
    "version": "1.0",
    "method": "thing.activate.info",
    "params":{
        "assetId": "12344",
        "productKey": "1234556554",
        "deviceKey": "deviceKey1234",
        "deviceSecret": "xxxxxx"
    }
}
```

设备在后续的接入EnOS的过程中，使用`productKey`、`deviceKey`、及`deviceSecret`以认证。认证过程与静态激活认证相同。

<!--end-->
