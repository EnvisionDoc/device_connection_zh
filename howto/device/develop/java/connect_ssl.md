# 通过SSL/TLS连接到EnOS云端

为了确保设备安全，你可以启用基于证书的认证，确保设备可以通过SSL/TLS连接到云端。你可以通过调用EnOS证书服务运用设备证书，将证书加载到SDK目录，然后通过SSL端口连接到服务器。服务器URL的格式为`ssl://{regionUrl}:18883`。参见下方的代码示例。

```java
client = new MqttClient(betaSSL, productKey, deviceKey, deviceSecret);
client.getProfile().setSSLSecured(true).setSSLJksPath("path.jks", "password");
client.connect(new IConnectCallback() {
    @Override
    public void onConnectSuccess() {
        System.out.println("connect success");
    }

    @Override
    public void onConnectLost() {
        System.out.println("onConnectLost");
    }

    @Override
    public void onConnectFailed(int reasonCode) {
        System.out.println("onConnectFailed :" + reasonCode);
    }
});
```

你还可以使用`setSSLContext()`方法直接设置SSL情境，加载证书内容，并使用基于证书的双向认证完成MQTT客户端的初始化。
