# 连接到EnOS云端

使用以下代码将设备连接到EnOS云端。

在以下示例中：
- `serverUrl`是指服务器地址。如果使用TCP连接，服务器URL的格式可以是`tcp://{regionUrl}:11883`。
- <`productKey`、`deviceKey`、`deviceSecret`>（或当拟采用动态方式激活设备时的<`productKey`、`deviceKey`、`productSecret`>）是指你注册设备时由EnOS发布的设备凭证。有关设备注册的更多信息，参见[将智能设备接入EnOS云端](/docs/device-connection/en/latest/quickstart/gettingstarted_device_connection)。


```java
MqttClient client = new MqttClient(serverUrl, productKey, deviceKey, deviceSecret);
client.connect(new IConnectCallback()
{
	@Override
	public void onConnectSuccess()
	{
		System.out.println("onConnectSuccess");
	}

	@Override
	 public void onConnectLost()
	{
		System.out.println("onConnectLost");
	}

	@Override
	public void onConnectFailed(int reasonCode)
	{
		System.out.println("onConnectFailed");
	}
});
```
