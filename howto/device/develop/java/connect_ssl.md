# 通过SSL/TLS连接到EnOS云端

为了确保设备安全，你可以启用基于证书的认证，确保设备可以通过SSL/TLS连接到云端。你可以通过调用EnOS证书服务运用设备证书，将证书加载到SDK目录，然后通过SSL端口连接到服务器。服务器URL的格式为`ssl://{regionUrl}:18883`。参见下方的代码示例。

```java
package com.envision.energy.enos_mqtt_sample;


import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.internals.SignMethod;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.LoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.NormalDeviceLoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.profile.DefaultProfile;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class SecureConnectSample {
    private final static Logger LOG = LoggerFactory.getLogger(SecureConnectSample.class);

    /**
     * 参考文档 https://www.envisioniot.com/docs/device-connection/zh_CN/latest/learn/deviceconnection_authentication.html
     * 以了解如何创建SSL jks文件
     */
    private final static String SSL_JKS_PATH = "<SslPath>";
    private final static String SSL_PASSWORD = "<Password>";

    private final static String SSL_SERVER_URL = "ssl://{regionUrl}:18883";

    private final static String PRODUCT_KEY = "<productKey>";
    private final static String DEVICE_KEY = "<deviceKey>";
    private final static String DEVICE_SECRET = "<deviceSecret>";

    public static void main(String[] args) {

        LoginInput input = new NormalDeviceLoginInput(SSL_SERVER_URL, PRODUCT_KEY, DEVICE_KEY, DEVICE_SECRET);
        final MqttClient client = new MqttClient(new DefaultProfile(input));
        client.getProfile()
                .setKeepAlive(300)
                .setAutoReconnect(true)
                .setSignMethod(SignMethod.SHA256)
                // 以下是SSL连接相关的设置
                .setSSLSecured(true)
                .setSSLJksPath(SSL_JKS_PATH, SSL_PASSWORD);

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



你还可以使用`setSSLContext()`方法直接设置SSL情境，加载证书内容，并使用基于证书的双向认证完成MQTT客户端的初始化。
