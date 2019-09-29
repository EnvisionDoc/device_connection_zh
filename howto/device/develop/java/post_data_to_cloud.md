# 发送数据到EnOS云端

连接成功后，即可从设备向EnOS发送数据。使用以下代码让设备发送数据至EnOS。

在以下示例中：

- serverUrl是指服务器地址。如果使用TCP连接，服务器URL的格式可以是`tcp://{regionUrl}:11883`。
- <productKey、deviceKey、deviceSecret>（或当拟采用动态方式激活设备时的<productKey、deviceKey、productSecret>）是指你注册设备时由EnOS发布的设备凭证。有关设备注册的更多信息，参见[将智能设备接入EnOS云端](/docs/device-connection/zh_CN/2.0.9/quickstart/gettingstarted_device_connection)。

```java
package com.envision.energy.enos_mqtt_sample;

import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.LoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.NormalDeviceLoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.profile.DefaultProfile;
import com.envisioniot.enos.iot_mqtt_sdk.message.upstream.tsl.MeasurepointPostRequest;
import com.envisioniot.enos.iot_mqtt_sdk.message.upstream.tsl.MeasurepointPostResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.Random;

public class MeasurePointPublishSample {
    private final static Logger LOG = LoggerFactory.getLogger(MeasurePointPublishSample.class);

    private final static String PLAIN_SERVER_URL = "tcp://{regionUrl}:11883";

    private final static String PRODUCT_KEY = "productKey";
    private final static String DEVICE_KEY = "deviceKey";
    private final static String DEVICE_SECRET = "deviceSecret";

    public static void main(String[] args) {
        LoginInput input = new NormalDeviceLoginInput(PLAIN_SERVER_URL, PRODUCT_KEY, DEVICE_KEY, DEVICE_SECRET);
        final MqttClient client = new MqttClient(new DefaultProfile(input));

        client.connect(new IConnectCallback() {
            public void onConnectSuccess() {
                LOG.info("onConnectSuccess");
                publishMeasurePoints(client);
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


    private static void publishMeasurePoints(final MqttClient client) {
        try {
            MeasurepointPostRequest request = MeasurepointPostRequest.builder()
                    .addMeasurePoint("sample.float_mp", new Random().nextFloat())
                    .addMeasurePoint("sample.string_mp", "test.string.mp")
                    .build();

            MeasurepointPostResponse response = client.publish(request);
            if (response.isSuccess()) {
                LOG.info("measure points are published successfully");
            } else {
                LOG.error("failed to publish measure points, error: {}", response.getMessage());
            }
        } catch (Exception e) {
            LOG.error("failed to publish measure points", e);
        }
    }
}
```

以下代码用于回调函数中子设备的登录操作。  

如果网关设备的网络连接中断，服务会将该网关设备拓扑中的所有子设备视为离线。尽管MQTT客户端允许自动重新连接，但当网络连接恢复时，将会调用`onConnectLost`回调函数。当重新连接生效时，子设备的在线逻辑将会被包含在`onConnectSuccess`回调方法中。

```java
@Override
public void onConnectSuccess()
{
    try
    {
        System.out.println("start register login sub-device , current status :" + client.isConnected());
        SubDeviceLoginRequest request = SubDeviceLoginRequest.builder().setSubDeviceInfo(subProductKey, subDeviceKey, subDeviceSecret).build();
        SubDeviceLoginResponse rsp = client.publish(request);
        System.out.println(rsp);
    }
    catch (Exception e)
    {
        e.printStackTrace();
    }
}
```

.. note:: 值得注意的是，子设备的`productKey`应通过EnOS控制台或EnOS REST API进行检索。`deviceKey`和`deviceSecret`还可通过SDK的`DeviceRegisterRequest`进行注册。

现在，使用以下示例代码发送子设备的测量点数据发送到云端。

```java
public static void postMeasurepoint()
{
      Map<String, String> struct = new HashMap<>();
      struct.put("structKey", "structValue");
      MeasurepointPostRequest request = MeasurepointPostRequest.builder()
                     .setProductKey(subProductKey).setDeviceKey(subDeviceKey)
                     .addMeasurePoint("p1", "string")
                     .addMeasreuPointWithQuality("p2", "value", 2)
                     .addMeasurePoint("p3", 100.2)
                     .addMeasurePoint("p4", struct)
                     .build();
      client.fastPublish(request);
}
```

在该示例中，使用了`fastPublish`方法。如此一来，便不会为测量点提供响应。在客户端中，还有另外2个发布方法。

```java
/**
 * publish the sync request and wait for the response
 * @throws Exception
 */
public <T extends IMqttResponse> T publish(IMqttRequest<T> request) throws Exception

/**
 * publish the request and register the callback , the callback will be called when rcv the response
 * @throws Exception
 */
public <T extends IMqttResponse> void publish(IMqttRequest<T> request, IResponseCallback<T> callback)
```

两者的不同之处在于：

- 带有回调函数的发布方法是异步方法。
- 带有响应参数的发布方法是同步方法。

需注意的是，如果`MeasurepointPostRequest`错误地调用了推送命令，服务就不会响应任何错误信息，直至会话超时为止。

设备端提供一个API，用于高质量地上传测量点数据，你可以使用该API高质量地发送测量点数据。
<!--该API的名称是什么？-->
