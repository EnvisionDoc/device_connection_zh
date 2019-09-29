# 与EnOS Cloud建立连接

本文介绍如何通过MQTT协议建立从设备到EnOS Cloud的连接。

支持的MQTT版本：

- 如果你使用基于证书的双向认证，在端口18883上使用 MQTT v3.1.1。
- 如果你使用基于密钥的单向认证，在端口11883上使用 MQTT v3.1.1。

## 直接使用MQTT协议

您可以直接使用MQTT协议将设备连接到EnOS Cloud。设备的CONNECT数据包中包含以下值：

```java
  mqttClientId: {clientId}|securemode={secureMode},signmethod=sha256,timestamp={timeStamp}|
  mqttUsername: {deviceKey}&{productKey}
  mqttPassword: toUpperCase(sha256({content}{deviceSecret}/{productSecret}))
```

- **mqttClientId** 部分: 由下列字段按照前述方式拼接而成：

  - `clientId`: 变量，必填项。可指定使用MAC地址或设备序列号。不可超过64个字符。

  - `securemode`: 必填项。表示所使用的安全模式。
    - 使用静态激活认证方式的设备，`securemode=2`
    - 使用动态激活认证方式的设备，`securemode=3`
  - `signmethod`: 必填项，当前支持sha256。表示使用SHA256签名算法。
  - `timestamp`: 变量，必填项。表示当前的时间的UNIX时间戳，单位为毫秒。

   例如，`mqttClientId`所需的各字段如下所示：
   - `clientId`=id123456
   - `securemode`=2，即采用静态激活模式
   - `sighmethod`=sha256
   - `timestamp`=1234567890
 
   则`mqttClientId`= `clientIdid123456|securemode=2,signmethod=sha256,timestamp=1234567890|`


- **mqttUsername** 部分: 由`deviceKey`、 “&”、 `productKey`三个要素拼接而成：

  - `deviceKey` ：变量，设备的device key，可以在EnOS控制台上获取。
  - `productKey`：变量，设备的product key，可以在EnOS控制台上获取。

  例如，某设备的`deviceKey`为`abcdefg`，`productKey`为`1234567`。则此处的`mqttUsername`为`abcdefg&1234567`

- **mqttPassword** 部分: 静态激活的设备，由`content`与`deviceSecret`拼接而成；动态激活的设备，由`content`与`productSecret`拼接而成。然后将拼接得来的字符串用SHA256算法计算出新的字符串，再将新的字符串的字母全部转换成大写字母。

  <!--- **mqttPassword** 可以由[password小工具](../../`static/nonsdk`enosmqttsign`index.html)生成，传入指定的参数可以自动生成。-->

  - `content`：按照 `clientID` , `deviceKey`, `productKey`, `timestamp`的顺序，将字段名称和值串联组合。
    
    例如：设备的参数值如下所示
      - `clientId`=id123456
      - `deviceKey`=dK987654
      - `productKey`=pK11111
      - `timestamp`=1234567890
    
    则 `content`= `clientIdid123456deviceKeydK987654productKeypK11111timestamp1234567890`

  - `deviceSecret`：设备的device secret。当设备完成创建后，可在EnOS Console上查看该值。
  - `productSecret`：设备的product secret。当设备完成创建后，可在EnOS Console上查看该值。


### 静态激活

静态激活认证过程中，`productKey`、`deviceKey`、和 `deviceSecret` 都已经在设备尝试认证并登陆EnOS之前，配置在了设备端。在 **设备管理 > 设备管理**中创建设备之后，你就可以获取`productKey`、`deviceKey`、和 `deviceSecret`。

对于静态激活认证方式：
```java
mqttPassword: toUpperCase(sha256({content}{deviceSecret}))
```

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
