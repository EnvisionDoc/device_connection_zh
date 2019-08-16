# 发送数据到EnOS云端

连接成功后，即可从设备向EnOS云端发送命令。举例来说，以下代码用于回调函数中子设备的登录操作。  

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
