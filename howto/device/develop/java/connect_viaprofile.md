# 使用配置文件连接到EnOS

除了使用[连接到EnOS云端](connect)中描述方式连接至服务器，SDK还提供了配置文件的方式进行连接，用户只需要提前配置一下配置文件，提供必要的参数信息。

## 配置文件示例

以下为配置文件内容的示例：

```java
//store config by mqtt sdk
//Mon Jan 14 20:39:20 CST 2019
{
  "serverUrl": "tcp://10.27.21.6:11883", //mandatory
  "productKey": "fKInRgVP", //mandatory
  "deviceKey": "zscai_test_activate",//mandatory
  "productSecret": "c3PW03srd6c",//needed when login by securemode=3
  "deviceSecret":"", //needed when login by securemode=2
  "operationTimeout": 60,//timeout seconds of each operation
  "keepAlive": 60,//conneciton keepAlive  seconds
  "connectionTimeout": 30, //connect timeout seconds
  "maxInFlight": 10000, //maxInFlight mqtt requests
  "autoReconnect": true,//auto reconnect the mqtt client when connect lost
  "sslSecured":false,//enable the ssl connection
  "sslAlgorithm": "SunX509",//ssl algorithm
  "sslJksPath": "",//path of jks file
  "sslPassword": "",//password of jks file
  "subDevices": //if subDevies is config, subdevice will auto login by sdk when mqtt connection build
   [
    {
      "productKey": "subProduct",
      "deviceKey": "subdevice",
      "deviceSecret": "xxx"
    },
    {
      "productKey": "subProduct",
      "deviceKey": "subdevice",
      "deviceSecret": "xxx"
    }
  ]
}
```

-  其中`regionURL`，`productKey`，`deviceKey`是必填参数，`deviceSecret`和`productSecret`必须提供一个参数，用于完成必要的参数签名校验。
    - 当配置文件中提供了参数`deviceSecret`则采用securemode=2进行登录。
    - 如果没有提供`deviceSecret`，而提供了`productSecret`那么采用securemode=3进行登录。

需要注意的是如果在配置文件中配置了子设备信息，那么SDK会在连接成功之后，自动登录子设备。这些子设备需要在云端预先配置成该网关设备的子设备。如果用户需要手动的管理子设备的状态，那么可以不将子设备配置在配置文件中，用户根据`SubDeviceLoginRequest`手动的管理子设备的上线。

同样的如果子设备登录方式使用securemode=3动态激活方式登录，那么云端会返回设备对应的设备密钥至设备端，设备端需要持久化保存该设备密钥信息，后续使用设备密钥进行登录签名认证。同样的java sdk已经封装了这一过程。

## 代码示例

使用配置文件方式连接服务的示例代码如下：

```java
package com.envision.energy.enos_mqtt_sample;


import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.internals.SignMethod;
import com.envisioniot.enos.iot_mqtt_sdk.core.profile.FileProfile;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.io.File;

public class ConfigFileBasedConnectExample {
    private final static Logger LOG = LoggerFactory.getLogger(ConfigFileBasedConnectExample.class);

    /**
     * 参考文档 /docs/device-connection/zh_CN/2.0.9/learn/deviceconnection_authentication.html
     * 以了解如何创建SSL jks文件
     */
    private final static String SSL_JKS_PATH = "<path>";
    private final static String SSL_PASSWORD = "<password>";

    private final static String SSL_SERVER_URL = "ssl://{regionUrl}:18883";

    private final static String PRODUCT_KEY = "<productKey>";
    private final static String DEVICE_KEY = "<deviceKey>";
    private final static String DEVICE_SECRET = "<deviceSecret>";

    private final static File CONFIG_FILE = new File(System.getProperty("java.io.tmpdir"), DEVICE_KEY + ".config");

    private static void persistConfigFile() throws Exception {
        final String absPath = CONFIG_FILE.getAbsolutePath();
        System.out.println("profile abs path: " + absPath);

        FileProfile profile = new FileProfile(absPath);
        profile.setProductKey(PRODUCT_KEY);
        profile.setDeviceKey(DEVICE_KEY);
        profile.setDeviceSecret(DEVICE_SECRET);
        profile.setSignMethod(SignMethod.SHA256);
        profile.setKeepAlive(300);
        profile.setAutoReconnect(true);

        // 使用SSL连接
        profile.setServerUrl(SSL_SERVER_URL);
        profile.setSSLSecured(true);
        profile.setSSLJksPath(SSL_JKS_PATH, SSL_PASSWORD);

        profile.persist();
    }

    public static void main(String[] args) throws Exception {
        if (!CONFIG_FILE.exists()) {
            persistConfigFile();
        }

        // 现在使用配置文件连接。这里我们无需
        // 再次显式提供配置
        FileProfile profile = new FileProfile(CONFIG_FILE.getPath());
        final MqttClient client = new MqttClient(profile);

        client.connect(new IConnectCallback() {
            public void onConnectSuccess() {
                LOG.info("onConnectSuccess");
                client.close();
            }

            public void onConnectLost() {
                LOG.info("onConnectLost");
                client.close();
            }

            public void onConnectFailed(int reason) {
                LOG.info("onConnectFailed");
                client.close();
            }
        });
    }
}

```


