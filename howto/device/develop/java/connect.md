# 连接到EnOS云端

使用以下代码将设备连接到EnOS云端。

在以下示例中：
- `serverUrl`是指服务器地址。如果使用TCP连接，服务器URL的格式可以是`tcp://{regionUrl}:11883`。
- <`productKey`、`deviceKey`、`deviceSecret`>（或当拟采用动态方式激活设备时的<`productKey`、`deviceKey`、`productSecret`>）是指你注册设备时由EnOS发布的设备凭证。有关设备注册的更多信息，参见[将智能设备接入EnOS云端](../../../../quickstart/gettingstarted_device_connection)。


```java
package com.envision.energy.enos_mqtt_sample;

import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.internals.SignMethod;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.LoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.NormalDeviceLoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.profile.DefaultProfile;
import com.google.common.base.Preconditions;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

public class ConnectSample {
    private final static Logger LOG = LoggerFactory.getLogger(ConnectSample.class);
    private final static String PLAIN_SERVER_URL = "tcp://{regionUrl}:11883";

    private final static String PRODUCT_KEY = "<productKey>";
    private final static String DEVICE_KEY = "<deviceKey>";
    private final static String DEVICE_SECRET = "<deviceSecret>";

    private final static boolean USE_ASYNC_WAY = true;

    public static void main(String[] args) {

        LoginInput input = new NormalDeviceLoginInput(PLAIN_SERVER_URL, PRODUCT_KEY, DEVICE_KEY, DEVICE_SECRET);
        final MqttClient client = new MqttClient(new DefaultProfile(input));

        // 如果云端和本地没有数据交流
        // 连接会被关闭
        client.getProfile().setKeepAlive(300);

        // 打开自动连接功能，在连接被关闭时自动重连。注意
        // 重连只会在首次连接成功的情况下才会发生
        client.getProfile().setAutoReconnect(true);

        // 采用SHA-256认证方法
        client.getProfile().setSignMethod(SignMethod.SHA256);

        if (USE_ASYNC_WAY) {
            /**
             * 采用异步连接方式时不会堵塞。
             * 当自动重连功能开启时，以下回调函数会在每次重连时被调用
             * 应避免多次初始化资源。
             */
            client.connect(new IConnectCallback() {
                public void onConnectSuccess() {
                    LOG.info("onConnectSuccess");
                    afterConnect(client);

                    // 没有更多数据时，应关闭MQTT客户端
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

            LOG.info("connection is not ready here");
        } else {
            /**
             * 如果成功则返回连接，否则连接会被丢弃。
             */
            try {
                client.connect();
                afterConnect(client);
            } catch (Exception e) {
                LOG.info("failed to connect: " + e.getMessage());
                client.close();
            }

        }
    }

    private static void afterConnect(final MqttClient client) {
        Preconditions.checkArgument(client.isConnected(), "connection must be ready here");
        LOG.info("connection is ready now");

        // 你可以在这里开始发布数据
        // .....
    }

}


```
