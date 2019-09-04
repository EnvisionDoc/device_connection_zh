# 教程：为同一模型的设备设置不同阈值的告警

告警的 _阈值_ 是指，在告警规则中设置的一个数值，当某个模型下的设备实例的测点值与阈值范围的关系满足告警规则的设定时，EnOS会按照事先配置好的相关参数发出告警。本文模拟了一个智能楼宇场景，介绍了如何为同类型的设备，根据具体场景要求，设置不同阈值的告警。

## 模拟场景介绍

本教程中，我们通过设备属性,为同类的设备定义不同的触发告警的阈值，然后在定义告警规则时，选择属性作为触发告警的条件，具体情境如下：

一幢智能楼宇中，使用两块同样款式的电流表，分别用于计量一个电冰箱和一个节能灯的实时通过电流，并在实时通过电流超过设备最大可承受电流值时发出告警。假定电冰箱和节能灯的最大可承受电流值分别是1000mA和70mA。

在该场景中，我们需要为同样款式的电流表，设置不同的告警阈值，以便每一块电表都能在实时通过电流超过各自测量的设备的最大可承受电流值时，向EnOS发出告警。如下图所示：

.. image:: ../../media/device_based_alert_scenario.png
   :width: auto

## 开始前准备

- 你需要有模型、设备管理、告警管理相关操作权限，如果没有请联系组织管理员添加，参见[策略，角色，与权限](/docs/iam/zh_CN/latest/access_policy)。

## 步骤1：设置开发环境

EnOS Java SDK for MQTT要求安装Java SE 8和Maven 3。按照以下步骤设置开发环境：

1. 安装JDK，下载地址：https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html。

2. 安装Maven，下载地址：http://maven.apache.org/download.cgi。

3. 安装开发环境，如IntelliJ IDEA，下载地址：https://www.jetbrains.com/idea/download/。

4. 在你的创建的project的pom.xml中，添加EnOS Java SDK for MQTT的依赖如下：

 ```xml
 <dependency>
     <groupId>com.envisioniot</groupId>
     <artifactId>enos-mqtt</artifactId>
     <version>2.1.2</version>
  </dependency>
 ```

## 步骤2： 创建电流表设备实例

创建模型的步骤，参考[创建模型](../model/creating_model)。模型的模型信息和功能定义如下：

 .. csv-table:: 模型信息
    :header: "字段","取值"
    :widths: auto

    "模型标识符", "current_meter"
    "模型名称", "电流表"
    "模型关系", "无"
    "模型模板", "无"
    "模型描述", "测量设备实时电流的设备"

 .. csv-table:: 功能定义
    :header: "字段","要素1","要素2"
    :widths: auto

    "功能类型", "属性", "测点"
    "名称", "最大允许通过电流", "实时电流"
    "标识符", "max_current", "rt_current"
    "数据类型", "double", "double"
    "单位", "电流：毫安 | mA", "电流：毫安 | mA"

创建好的模型如下图所示：

.. image:: ../../media/tutorial_alert_1.png

.. image:: ../../media/tutorial_alert_2.png

## 步骤3：为电流表创建产品

创建产品的步骤，参考[创建产品](../device/manage/creating_product)。创建产品需要的字段信息如下：

.. csv-table:: 产品信息
    :header: "字段","取值"
    :widths: auto

    "产品名称", "电流表"
    "节点类型", "设备"
    "设备模型", "电流表"
    "数据格式", "JSON"
    "证书双向认证", "禁用"
    "产品描述", "无"

## 步骤4：为电流表创建设备实例

创建设备实例的步骤，参考[注册设备](../device/manage/creating_device)。在本场景中，我们需要创建两个设备实例，分别代表测量冰箱和节能灯的电流表。其必要信息如下：

.. csv-table:: 设备定义
    :header: "字段","设备实例1", "设备实例2"
    :widths: auto

    "产品", "电流表", "电流表"
    "Device Key", "fridgeMeter", "fluorescentLampMeter"
    "设备名称", "冰箱电表", "节能灯电表"
    "时区/城市", "UTC +08:00", "UTC +08:00"
    "最大允许通过电流", "1000", "70"

创建好的两个设备实例如图所示：

.. image:: ../../media/tutorial_alert_3.png

.. image:: ../../media/tutorial_alert_4.png

.. image:: ../../media/tutorial_alert_5.png

## 步骤5： 为设备实例配置告警信息

为两个电流表设备实例依次定义以下要素：

1. 告警级别。创建告警级别的步骤，参见[创建告警级别](create_alert_severity)，以下为一个供参考的告警级别实例：

 .. image:: ../../media/tutorial_alert_6.png

2. 告警类型。创建告警类型的步骤，参见[创建告警类型](create_alert_type)，以下为一个供参考的告警类型实例：

 .. image:: ../../media/tutorial_alert_7.png

3. 告警内容。创建告警内容的步骤，参见[创建告警内容](create_alert_content)，以下为一个供参考的告警内容实例：

 .. image:: ../../media/tutorial_alert_8.png

4. 告警规则。创建告警规则的步骤，参见[创建告警规则](create_alert_rule)，**触发条件** 选择以设备属性值作为告警阈值：

 .. image:: ../../media/tutorial_alert_9.png

## 步骤6：使用EnOS Java SDK for MQTT模拟物理设备发送数据

在Java IDE中创建两个Java class，分别名为 `fridgeMQTT` 和 `lampMQTT`。在示例代码中，虚拟冰箱和节能灯向EnOS发送的数据范围分别是[0,1500)毫安和[0,150)毫安。因此两个设备都有一定概率发送超过设备 **最大允许通过电流** 的数据，会触发告警。复制示例代码并将下列信息根据实际情况进行替换，然后运行代码。

- `env`：替换为自己使用的EnOS环境地址。
- `productKey`：替换为 “电流表” 产品的product key。
- `deviceSecret`：`fridgeMQTT`的`deviceSecret`替换成 **冰箱电表** 的device secret；`lampMQTT`的`deviceSecret`替换成 **节能灯电表** 的device secret。


`fridgeMQTT`的代码如下：

```java
import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.exception.EnvisionException;
import com.envisioniot.enos.iot_mqtt_sdk.message.upstream.tsl.MeasurepointPostRequest;
import com.envisioniot.enos.iot_mqtt_sdk.sample.SimpleSendReceive;

import java.util.Random;

public class fridgeMQTT {
    public static final String env = "tcp://url:port";
    public static final String productKey = "productKey";
    public static final String deviceKey = "fridgeMeter";
    public static final String deviceSecret = "deviceSecret";
    private static MqttClient client;
    private static volatile boolean subDeviceLogined = false;
    private static Random random = new Random();
    private static int idInc = 20;
    private static final char[] HEX_CHAR = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};

    public fridgeMQTT() {
    }

    public static void main(String[] args) throws Exception {
        initWithCallback();
        alwaysPostMeasurepoint();
    }

    public static void initWithCallback() {
        System.out.println("start connect with callback ... ");

        try {
            client = new MqttClient(env, productKey, deviceKey, deviceSecret);
            client.getProfile().setConnectionTimeout(60).setAutoReconnect(false);
            client.connect(new IConnectCallback() {
                public void onConnectSuccess() {
                    SimpleSendReceive.subDeviceLogin();
                    System.out.println("connect success");
                }

                public void onConnectLost() {
                    System.out.println("onConnectLost");
                }

                public void onConnectFailed(int reasonCode) {
                    System.out.println("onConnectFailed : " + reasonCode);
                }
            });
        } catch (EnvisionException var1) {
        }

        System.out.println("connect result :" + client.isConnected());
    }

    public static void alwaysPostMeasurepoint() throws Exception {
        while(true) {
            long ts = System.currentTimeMillis();
            Random random = new Random();
            System.out.println("start post measurepoint ...");

            MeasurepointPostRequest request = MeasurepointPostRequest.builder().addMeasurePoint("rt_current", random.nextDouble() * 1500).build();

            try {
                client.fastPublish(request);
                System.out.println(" post measurepoint success...");
            } catch (Exception var3) {
                var3.printStackTrace();
            }
            System.out.println(client.isConnected() + " post  cost " + (System.currentTimeMillis() - ts) + " millis");
            Thread.sleep(10000L);
        }
    }
}
```
`lampMQTT`的示例代码如下：
```java
import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
import com.envisioniot.enos.iot_mqtt_sdk.core.exception.EnvisionException;
import com.envisioniot.enos.iot_mqtt_sdk.message.upstream.tsl.MeasurepointPostRequest;
import com.envisioniot.enos.iot_mqtt_sdk.sample.SimpleSendReceive;

import java.util.Random;

public class lampMQTT {
    public static final String env = "tcp://url:port";
    public static final String productKey = "productKey";
    public static final String deviceKey = "fridgeMeter";
    public static final String deviceSecret = "deviceSecret";
    private static MqttClient client;
    private static volatile boolean subDeviceLogined = false;
    private static Random random = new Random();
    private static int idInc = 20;
    private static final char[] HEX_CHAR = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};

    public fridgeMQTT() {
    }

    public static void main(String[] args) throws Exception {
        initWithCallback();
        alwaysPostMeasurepoint();
    }

    public static void initWithCallback() {
        System.out.println("start connect with callback ... ");

        try {
            client = new MqttClient(env, productKey, deviceKey, deviceSecret);
            client.getProfile().setConnectionTimeout(60).setAutoReconnect(false);
            client.connect(new IConnectCallback() {
                public void onConnectSuccess() {
                    SimpleSendReceive.subDeviceLogin();
                    System.out.println("connect success");
                }

                public void onConnectLost() {
                    System.out.println("onConnectLost");
                }

                public void onConnectFailed(int reasonCode) {
                    System.out.println("onConnectFailed : " + reasonCode);
                }
            });
        } catch (EnvisionException var1) {
        }

        System.out.println("connect result :" + client.isConnected());
    }

    public static void alwaysPostMeasurepoint() throws Exception {
        while(true) {
            long ts = System.currentTimeMillis();
            Random random = new Random();
            System.out.println("start post measurepoint ...");

            MeasurepointPostRequest request = MeasurepointPostRequest.builder().addMeasurePoint("rt_current", random.nextDouble() * 150).build();

            try {
                client.fastPublish(request);
                System.out.println(" post measurepoint success...");
            } catch (Exception var3) {
                var3.printStackTrace();
            }
            System.out.println(client.isConnected() + " post  cost " + (System.currentTimeMillis() - ts) + " millis");
            Thread.sleep(10000L);
        }
    }
}
```

## 结果

进入 **告警管理 > 告警记录** ，在 **筛选** 中，将 **模型** 设置为 **电流表**， 点击 **搜索** 。 此时可以看到两台虚拟设备超出告警规则设置的阈值的测点数据，触发了 **Current above threshold** 的告警。

.. image:: ../../media/tutorial_alert_10.png








