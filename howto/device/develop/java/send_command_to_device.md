# 发送命令到设备

该文章介绍了如何处理从EnOS云端发送到设备的下游命令。在设备端，可在`onMessage`回调函数中配置响应信息，这样SDK便可响应云端的消息。你可以配置错误码和错误消息，用以向设备返回指令报错。对于此类自定义错误代码和错误消息，你可以选择使用2000或更大的数字。

在以下代码示例中，会对设置测量点和禁用的设备进行监控。

```java
package com.envision.energy.enos_mqtt_sample;

import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.LoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.login.NormalDeviceLoginInput;
import com.envisioniot.enos.iot_mqtt_sdk.core.msg.IMessageHandler;
import com.envisioniot.enos.iot_mqtt_sdk.core.profile.DefaultProfile;
import com.envisioniot.enos.iot_mqtt_sdk.message.downstream.tsl.ServiceInvocationCommand;
import com.envisioniot.enos.iot_mqtt_sdk.message.downstream.tsl.ServiceInvocationReply;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import java.util.List;
import java.util.Map;
import java.util.Timer;
import java.util.TimerTask;

public class ServiceInvokeExample {
    private final static Logger LOG = LoggerFactory.getLogger(ServiceInvokeExample.class);

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

                // Set service handler to handle service command from cloud
                client.setArrivedMsgHandler(ServiceInvocationCommand.class, createServiceCommandHandler(client));
                LOG.info("waiting commands from cloud");
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

    private static IMessageHandler<ServiceInvocationCommand, ServiceInvocationReply> createServiceCommandHandler(final MqttClient client) {
        return (ServiceInvocationCommand request, List<String> argList) -> {
            LOG.info("receive command: {}", request);

            // argList: productKey, deviceKey, serviceName
            // If the request is for sub-device, the productKey and deviceKey
            // are used to identify the target sub-device.
            String productKey = argList.get(0);
            String deviceKey = argList.get(1);
            String serviceName = argList.get(2);
            LOG.info("productKey: {}, deviceKey: {}, serviceName: {}, params: {}",
                    productKey, deviceKey, serviceName, request.getParams());

            if (serviceName.equals("sample.echo")) {
                Map<String, Object> params = request.getParams();
                String message = (String) params.get("message");
                LOG.info("arg message: {}", message);

                // Set the reply result
                return ServiceInvocationReply.builder().addOutputData("reply", message).build();
            } else if (serviceName.equals("sample.disconnect")) {
                Map<String, Object> params = request.getParams();
                int delayMS = (Integer) params.get("delayMS");
                LOG.info("arg delay: {}", delayMS);

                final Timer timer = new Timer();
                timer.schedule(new TimerTask() {
                    @Override
                    public void run() {
                        LOG.info("now close connection ...");
                        client.close();
                        timer.cancel();
                    }
                }, delayMS);
                return ServiceInvocationReply.builder().build();
            }

            return ServiceInvocationReply.builder().setMessage("unknown service: " + serviceName).setCode(220).build();
        };
    }
}

```


.. note:: 如果用户响应了一个空回复，设备端在执行回调方法后将不会作出响应。如果用户响应了一个有效回复，SDK将会对该回复进行序列化处理并自动将其发送到云端。如果成功执行了回调方法，用户便可将响应代码设置为200或非200。如果执行失败，用户可以将自定义错误代码设置为2000或更大的数字，然后发送到云端。

<!--结束-->
