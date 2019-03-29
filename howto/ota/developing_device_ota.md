# 开发设备端OTA能力

## 任务背景信息

设备端OTA升级很大程度上依赖于设备自身的物理空间和功能实现。针对硬件开发者，EnOS平台提供包含OTA能力的Device SDK，支持以下功能：

- 设备上报固件版本信息：设备与云端建连时上报一次自身的版本信息，云端记录设备的版本号；
- 设备接收云端升级推送请求：云端推送升级请求后，设备可接收到升级请求并下载固件文件；
- 设备向云端主动请求可升级的固件版本：支持设备主动向云端请求，并查询可升级的版本信息；
- 设备上报升级进度和升级失败的错误原因：设备进入升级流程后，可向云端上报升级的进度或升级失败的原因，方便运维人员进行远程升级管理；

.. note:: 一般而言，设备的OTA升级需要具备相应的物理条件，EnOS平台提供设备OTA升级所需的云端消息通道和文件下载服务，并提供设备端SDK方便开发者调用接口能力，而真正的设备端固件升级则依赖于设备端Flash是否具备足够的物理空间，开发者需自行实现文件下载后的固件切换、引导和重启等业务逻辑。


### 固件升级TOPIC

EnOS云端提供的固件升级的消息topic，升级请求支持使用MQTT进行消息的发送和订阅：

1. 设备端上报固件版本给云端：设备端发布权限，用于设备与云端每次建连成功后上报当前的版本号信息

   请求topic: `/sys/${productkey}/${devicekey}/ota/device/inform`

   响应topic: `/sys/${productkey}/${devicekey}/ota/device/inform_reply`

2. 设备端接收云端OTA请求：设备端订阅权限，用于接收云端推送的OTA升级通知，包含固件名称、版本号、MD5和下载URL等信息

   请求topic: `/sys/${productkey}/${devicekey}/ota/device/upgrade`

   响应topic:  `/sys/${productkey}/${devicekey}/ota/device/upgrade_reply`

3. 设备端上报固件升级进度：设备通过HTTPS循环请求固件下载，并通过这个topic上报固件升级事件、错误码和下载进度的百分比等信息

   请求topic: `/sys/${productkey}/${devicekey}/ota/device/progress`

   响应topic: ` /sys/${productkey}/${devicekey}/ota/device/progress_reply`

4. 设备主动请求可升级的固件版本：云端创建一个OTA升级策略为“设备请求升级”，设备可通过这个topic发布升级请求，接收云端返回的可用的升级版本信息。

   请求topic: `/sys/${productkey}/${devicekey}/ota/device/getversion`

   响应topic:  `/sys/${productkey}/${devicekey}/ota/device/getversion_reply`


## 开始前准备

- 了解EnOS的OTA升级流程，参见[设备固件升级概述](ota_overview).

## 步骤1 安装Device SDK

下载[enos-mqtt-sdk-java](https://github.com/EnvisionIot/enos-mqtt-sdk-java)。如果使用Maven，其依赖如下：

```
<dependency>
  <groupId>com.envisioniot</groupId>
  <artifactId>enos-mqtt</artifactId>
  <version>2.1.2</version>
</dependency>
```

## 步骤2：下载设备固件

   - 通过REST API `getFile`下载，查看API文档的步骤为：在EnOS控制台导航栏中点击**EnOS API > API文档 > Common File Service > getFile**.

   - 通过云端服务SDK
     - [Java SDK](https://github.com/EnvisionIot/enos-api-sdk-java)
     - [python SDK](https://github.com/EnvisionIot/enos-api-sdk-python)

## 示例代码

以下为通过Java SDK进行设备端OTA能力开发的示例代码：

```java
import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.exception.EnvisionException;
import com.envisioniot.enos.iot_mqtt_sdk.core.msg.IMessageHandler;
import com.envisioniot.enos.iot_mqtt_sdk.core.msg.IMqttDeliveryMessage;
import com.envisioniot.enos.iot_mqtt_sdk.core.profile.DefaultProfile;
import com.envisioniot.enos.iot_mqtt_sdk.message.downstream.ota.OtaUpgradeCommand;
import com.envisioniot.enos.iot_mqtt_sdk.message.upstream.ota.*;

import java.util.List;
import java.util.concurrent.TimeUnit;

/**
 * device firmware over-the-air
 */
public class OtaSample {

    //Please use your own productKey, deviceKey, and deviceSecret
    static String productKey = "testPK";
    static String deviceKey = "testDK";
    static String deviceSecret = "testDS";

    //Use the actual mqtt-broker url
    static String brokerUrl = "tcp://{mqtt-broker-url}";

    static MqttClient client;

    public static void main(String[] args) throws Exception {
        client = new MqttClient(new DefaultProfile(brokerUrl, productKey, deviceKey, deviceSecret));
        initWithCallback(client);

        //Report firmware version first
        reportVersion("initVersion");

        //        upgradeFirmwareByCloudPush();


        //        upgradeFirmwareByDeviceReq();
    }

    public static void upgradeFirmwareByCloudPush() {
        client.setArrivedMsgHandler(OtaUpgradeCommand.class, new IMessageHandler<OtaUpgradeCommand, IMqttDeliveryMessage>() {
            @Override
            public IMqttDeliveryMessage onMessage(OtaUpgradeCommand otaUpgradeCommand, List<String> list) throws Exception {
                System.out.println("receive command: " + otaUpgradeCommand);

                Firmware firmware = otaUpgradeCommand.getFirmwareInfo();

                //TODO: download firmware from firmware.fileUrl

                //Mock reporting progress
                reportUpgradeProgress("20", "20");
                TimeUnit.SECONDS.sleep(2);

                reportUpgradeProgress("25", "25");
                TimeUnit.SECONDS.sleep(20);

                reportUpgradeProgress("80", "80");
                TimeUnit.SECONDS.sleep(20);

                //Firmware upgrade success, report new version
                reportVersion(otaUpgradeCommand.getFirmwareInfo().version);

                return null;
            }
        });
    }

    public static void upgradeFirmwareByDeviceReq() throws Exception {
        List<Firmware> firmwareList = getFirmwaresFromCloud();
        String version = null;
        for (Firmware firmware : firmwareList) {
            version = firmware.version;
            StringBuffer sb = new StringBuffer();
            sb.append("Firmware=>[");
            sb.append("version=" + firmware.version);
            sb.append("signMethod=" + firmware.signMethod);
            sb.append("sign=" + firmware.sign);
            sb.append("fileUrl=" + firmware.fileUrl);
            sb.append("fileSize=" + firmware.fileSize);
            sb.append("]");
            System.out.println(sb.toString());
        }
        if (version != null) {
            reportUpgradeProgress("20", "20");
            TimeUnit.SECONDS.sleep(10);
            reportUpgradeProgress("80", "80");
            TimeUnit.SECONDS.sleep(20);
            reportVersion(version);
        }
    }

    public static void reportVersion(String version) throws Exception {
        OtaVersionReportRequest.Builder builder = new OtaVersionReportRequest.Builder();
        builder.setProductKey(productKey).setDeviceKey(deviceKey).setVersion(version);
        OtaVersionReportRequest request = builder.build();
        System.out.println("send =>" + request.toString());
        client.fastPublish(builder.build());
    }

    private static void reportUpgradeProgress(String progress, String desc) throws Exception {
        OtaProgressReportRequest.Builder builder = new OtaProgressReportRequest.Builder();
        builder.setStep(progress).setDesc(desc);
        client.fastPublish(builder.build());
    }

    private static List<Firmware> getFirmwaresFromCloud() throws Exception {
        OtaGetVersionRequest.Builder builder = new OtaGetVersionRequest.Builder();
        builder.setProductKey(productKey).setDeviceKey(deviceKey);
        OtaGetVersionRequest request = builder.build();
        OtaGetVersionResponse response = client.publish(request);
        System.out.println("send getversion request =>" + request.toString());
        System.out.println("receive getversion response =>" + response.toString());
        return response.getFirmwareList();
    }

    private static void initWithCallback(MqttClient client) {
        System.out.println("start connect with callback ... ");
        try {
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
                    System.out.println("onConnectFailed : " + reasonCode);
                }

            });
        } catch (EnvisionException e) {
            e.printStackTrace();
        }
    }
}
```

## 固件升级协议

### 设备端上报版本号

设备端可发送当前的固件版本号给云端，只有上报过固件版本号的设备，才能在控制台创建升级任务。

上行TOPIC

- 请求topic: `/sys/${productkey}/${devicekey}/ota/device/inform`
- 响应topic: `/sys/${productkey}/${devicekey}/ota/device/inform_reply`

请求数据格式：

```JSON
{
    "id":"123",
    "version":"1.0",
    "method":"ota.device.inform",
    "params": {
        "version":"xxxxxxxx"
    }
}
```

响应数据格式：

```JSON
{
    "id": "123",
    "code": 200,
    "data": {}
}
```

参数说明：

.. list-table::
   :widths: auto

   * - 参数
     - 类型
     - 是否必须
     - 描述
   * - id
     - Long
     - 可选
     - 消息ID，保留值
   * - version
     - String
     - 必需
     - 协议版本号，目前协议版本1.0
   * - method
     - String
     - 必需
     - 请求方法，必需为ota.device.inform
   * - params
     - Object
     - 必需
     - 上报版本号参数
   * - version
     - String
     - 必需
     - 上报的版本号
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行

### 设备接收云端请求

在控制台创建云端推送的固件升级任务，符合升级条件的在线设备，将会收到云端推送过来的固件信息，如果当前设备不在线，将在下次上线时接收到升级请求。

下行TOPIC：

- 请求topic: `/sys/${productkey}/${devicekey}/ota/device/upgrade`
- 响应:  `/sys/${productkey}/${devicekey}/ota/device/upgrade_reply`

请求数据格式：
```JSON
{
    "id":"123",
    "version":"1.0",
    "method":"ota.device.upgrade",
    "params": {
        "version":"v1.0",
        "sign":"xxxxxxx",
        "signMethod":"md5",
        "fileUrl":"/1b30e0ea83002000/ota/kadak13",
        "fileSize":1024
    }
}
```

响应数据格式：
```JSON
{
    "id": "123",
    "code": 200,
    "data": {}
}
```

参数说明：

.. list-table::
   :widths: auto

   * - 参数
     - 类型
     - 是否必须
     - 描述
   * - id
     - Long
     - 可选
     - 消息ID，保留值
   * - version
     - String
     - 必需
     - 协议版本号，目前协议版本1.0
   * - method
     - String
     - 必需
     - 请求方法，必需为ota.device.getversion
   * - params
     - Object
     - 可选
     - 无
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求执行成功
   * - data
     - List
     - 可选
     - 返回的详细信息，JSON格式
   * - version
     - String
     - 必需
     - 固件版本号
   * - sign
     - String
     - 必需
     - 固件签名
   * - signMethod
     - String
     - 必需
     - 固件签名算法
   * - fileUrl
     - String
     - 必需
     - 固件文件url
   * - fileSize
     - Long
     - 必需
     - 固件文件大小，单位：字节

### 设备上报固件升级进度

设备开始下载固件后，可以通过此topic上报升级进度，或上报升级错误的相关原因，运维人员可以在控制台中查看升级进度。

上行TOPIC：
- 请求TOPIC：`/sys/${productkey}/${devicekey}/ota/device/progress`
- 响应TOPIC：`/sys/${productkey}/${devicekey}/ota/device/progress_reply`

请求数据格式：
```JSON
{
    "id":"123",
    "version":"1.0",
    "method":"ota.device.progress",
    "params": {
        "step":"1",
        "desc":"xxxxxxxx"
    }
}
```

响应数据格式：
```JSON
{
    "id": "123",
    "code": 200,
    "data": {}
}
```

参数说明：

.. list-table::
   :widths: auto

   * - 参数
     - 类型
     - 是否必须
     - 描述
   * - id
     - Long
     - 可选
     - 消息ID，保留值
   * - version
     - String
     - 必需
     - 协议版本号，目前协议版本1.0
   * - method
     - String
     - 必需
     - 请求方法，必需为ota.device.process
   * - params
     - Object
     - 必需
     - 上报进度参数
   * - step
     - String
     - 必需
     - 上报的进度，固件升级进度比，正常范围为[0, 100]，升级失败 -1：代表升级失败, -2：代表下载失败, -3：代表校验失败, -4：代表烧写失败
   * - desc
     - String
     - 可选
     - 当前步骤的描述信息，如果发生异常可以用此字段承载错误信息
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行


### 设备主动请求升级

对于非强制升级的场景，可以在控制台创建设备主动请求升级的升级任务，此时云端不会主动推送升级请求，而是等待设备主动请求，以获取可升级的版本信息，如果云端存在多个版本信息，则返回一个可升级版本的list，由设备或设备所有者选择进行升级。


上行TOPIC

- 请求topic: `/sys/${productkey}/${devicekey}/ota/device/getversion`
- 响应topic:  `/sys/${productkey}/${devicekey}/ota/device/getversion_reply`

请求数据格式

```JSON
{
    "id":"123",
    "version":"1.0",
    "method":"ota.device.getversion",
    "params": {
    }
}
```

响应数据格式：

```JSON
{
    "id":"123",
    "code":200,
    "data": [
         {
            "version":"v1.0",
            "sign":"xxxxxxx",
            "signMethod":"md5",
            "fileUrl":"/1b30e0ea83002000/ota/kadak13",
            "fileSize":1024
        }
    ]
}
```


参数说明：

.. list-table::
   :widths: auto

   * - 参数
     - 类型
     - 是否必须
     - 描述
   * - id
     - Long
     - 可选
     - 消息ID，保留值
   * - version
     - String
     - 必需
     - 协议版本号，目前协议版本1.0
   * - method
     - String
     - 必需
     - 请求方法，必需为ota.device.getversion
   * - params
     - Object
     - 可选
     - 无
   * - code
     - Integer
     - 必需
     - 结果返回码，200代表请求成功执行
   * - data
     - List
     - 可选
     - 返回的详细信息，JSON格式，如果该设备在云端存在多个可升级的版本，将返回多版本信息
   * - version
     - String
     - 必需
     - 固件版本号
   * - sign
     - String
     - 必需
     - 固件签名
   * - signMethod
     - String
     - 必需
     - 固件签名算法
   * - fileUrl
     - String
     - 必需
     - 固件签名算法
   * - fileSize
     - Long
     - 必需
     - 固件文件大小，单位：字节


### 异常情况处理

对于固件升级过程的异常情况，可以在上报固件升级进度时，通过参数step和desc来表示异常情况。step小于0，表示异常情况，desc则是对异常情况的描述。

目前系统提供的异常情况step如下：

.. list-table::
   :widths: auto

   * - step值
     - 含义
   * - -1
     - 升级失败
   * - -2
     - 下载失败
   * - -3
     - 校验失败
   * - -4
     - 烧写失败

若有其它自定义异常，可设置step为小于-4的数字，desc设置成相应的异常情况描述即可。
