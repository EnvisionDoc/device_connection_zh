# 将智能设备连接至EnOS Cloud

该教程将指导你如何通过Java SDK模拟智能设备直连入EnOS Cloud，在设备与EnOS Cloud之间发送数据，并从EnOS Console查看设备通信信息。

## 场景描述<scenario>

接入场景参考[设备接入方案](../learn/connection_scenarios)当中提到的“场景1.1”。


## 任务描述<description>

本示例以用户光伏逆变器接入为例进行说明，逆变器采集器出厂烧录逆变器设备三元组。逆变器上电、联网以后，基于设备三元组认证直连云端IoT Hub。整体流程如下图所示：

.. image:: ../media/device_connection_task_description.png


基于上述接入流程图，本示例主要有以下任务：

1. 创建设备模型

2. 创建产品

3. 注册设备

4. 通过设备SDK模拟设备发送数据

5. 查看设备通信状态

6. 查看设备数据

## 步骤1：创建设备模型<createmodel>

该步骤假设没有可以复用的设备模型，我们创建一个名称为 **Inverter_Demo**，功能定义如下表的新的模型：

.. list-table::
   :widths: auto

   * - 功能类型
     - 名称   
     - 标识符   
     - 数据类型   
     - 数据定义   
   * - 属性
     - 逆变器类型     
     - invType
     - enum  
     - {0:Central,1:String}      
   * - 属性
     - 组件容量
     - capacity     
     - float
     - kWp      
   * - 测点
     - 有功功率    
     - INV.GenActivePW
     - double
     - kW      
   * - 服务
     - 控制     
     - INV.Control
     - --  
     - 调用方式:异步调用      
   * - 事件
     - 故障信息     
     - Error
     - --  
     - 事件类型:故障      

创建该模型的步骤如下：

1. 在EnOS控制台中选择 **模型**。

2. 点击在页面右上方 **创建模型**, 并在 **创建模型** 窗口提供以下配置信息：

   - **模型标识符**： Inverter_Demo
   - **模型名称**：Inverter_Demo
   - **分类**：无
   - **模型关系**：无
   - **模型模板**：无
   - **模型描述**：Inverter model for demo project

3. 点击 **确定** 完成操作。

   .. image:: ../media/model_inverter.png


4. 点击 **修改**，在模型详细信息界面中点击 **功能定义** 标签。

5. 点击 **新增**，并在 **添加功能** 窗口提供以下配置信息：

   - **属性1**
      - **名称**：逆变器类型/Inverter_Type
      - **标识符**：invType
      - **数据类型**：enum
      - **枚举项**：
        - 参数值：0；参数描述：Central
        - 参数值：1；参数描述：String
      - **是否必填**：是
   - **属性2**
      - **名称**：组件容量/Inverter_Capacity
      - **标识符**：capacity
      - **数据类型**：float
      - **单位**：kWp
      - **是否必填**：是
   - **测点**
      - **名称**：有功功率/Active_Power
      - **标识符**：INV.GenActivePW
      - **数据类型**：double
      - **测点类型**：AI
      - **单位**：kW
   - **服务**
      - **名称**：控制/Control
      - **标识符**：INV.Control
      - **调用方式**：异步
      - **输入参数**：
        - 参数名称：control
        - 标识符：control
        - 数据类型：enum
        - 枚举项：
          - 参数值：0；参数描述：Stop
          - 参数值：1；参数描述：Start
      - **输出参数**：
        - 参数名称：execResult
        - 标识符：execResult
        - 数据类型：enum
        - 枚举项：
          - 参数值：0；参数描述：Failure
          - 参数值：1；参数描述：Success
   - **事件**
      - **名称**：故障信息/Error
      - **标识符**：Error
      - **事件类型**：故障

有关设备模型设置的详细信息，参见[创建模型](../howto/model/creating_model)。

## 步骤2：创建产品<createproduct>

在该步骤，我们创建一个名为 **Inverter_Product** 的产品，我们假设该产品型号的设备通过JSON格式上送数据且数据传输不使用CA证书加密。

1. 在EnOS控制台中选择 **设备管理 > 产品管理**。

2. 点击在页面右上方 **创建产品**，在 **创建产品** 窗口提供以下配置信息。

   - **产品名称**：Inverter_Product
   - **节点类型**：设备
   - **设备模型**：Inverter_Demo
   - **数据格式**：Json
   - **证书双向认证**：禁用
   - **产品描述**：Inverter product for demo

3. 点击 **确定** 完成操作。

   .. image:: ../media/create_product.png


有关产品设置的详细信息，参见[创建产品](../howto/device/manage/creating_product)。


## 步骤3：注册设备<registerdevice>

在该步骤中，我们创建一个名称为 **INV001** 的设备，该设备属于在上一步骤中创建的 **Inverter_Product** 产品型号。

1. 在EnOS控制台中选择 **设备管理**。

2. 点击在页面右上方 **添加设备** ，在弹出窗口配置如下信息：

   - **产品**：Inverter_Product
   - **Device Key**：选填，系统自动生产
   - **设备名称**：INV001
   - **时区/城市**：UTC+14:00
   - **使用夏令时** ：否
   - **逆变器类型**：0:Central，表示集中式逆变器
   - **组件容量**：5.0

   .. image:: ../media/register_device.png

有关设备设置的详细信息，参见[创建设备](../howto/device/manage/creating_device)。

完成设备注册后，获取设备三元组`ProductKey`，`DeviceKey`和`DeviceSecret`，将在下一步中使用。

## 步骤4：SDK模拟设备发送数据<senddata>

在该步骤中，我们通过设备端SDK模拟发送逆变器有功功率至云端。

1. 获取[设备端SDK](https://github.com/EnvisionIot/enos-mqtt-java-sdk)。更多信息，参考该SDK的GitHub readme文件。

2. 配置EnOS Cloud连接地址。

3. 将Cloud端地址`environment_address`及注册设备获取的设备三元组（`ProductKey`,`DeviceKey`,`DeviceSecret`）配置至sample连接程序当中。设备三元组为注册设备步骤中获得。

4. 修改`initWithCallback`方法，将设备与云端建立连接。

5. 修改`postMeasurepoint()`方法，配置发送数据测点名称，这里发送逆变器有功功率点，设置点名 **INV.GenActivePW**，以及对应的点值。

   连接设备并模拟设备发送数据的代码示例如下：

   ```java
   import com.envisioniot.enos.iot_mqtt_sdk.core.IConnectCallback;
   import com.envisioniot.enos.iot_mqtt_sdk.core.MqttClient;
   import com.envisioniot.enos.iot_mqtt_sdk.core.exception.EnvisionException;
   import com.envisioniot.enos.iot_mqtt_sdk.message.upstream.tsl.MeasurepointPostRequest;
   import com.envisioniot.enos.iot_mqtt_sdk.sample.SimpleSendReceive;
   
   import java.util.Random;
   
   public class demo1 {
       public static final String url = "tcp://{environment_address}";
       public static final String productKey = "ProductKey";
       public static final String deviceKey = "{DeviceKey}";
       public static final String deviceSecret = "{DeviceSecret}";
       private static MqttClient client;
       private static volatile boolean subDeviceLogined = false;
       private static Random random = new Random();
       private static int idInc = 20;
       private static final char[] HEX_CHAR = new char[]{'0', '1', '2', '3', '4', '5', '6', '7', '8', '9', 'a', 'b', 'c', 'd', 'e', 'f'};
   
       public demo() {
       }
   
       public static void main(String[] args) throws Exception {
           initWithCallback();
           postMeasurepoint();
       }
   
       public static void initWithCallback() {
           System.out.println("start connect with callback ... ");
   
           try {
               client = new MqttClient(url, productKey, deviceKey, deviceSecret);
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
   
       public static void postMeasurepoint() {
           Random random = new Random();
           System.out.println("start post measurepoint ...");
           MeasurepointPostRequest request = (MeasurepointPostRequest)MeasurepointPostRequest.builder().addMeasurePoint("INV.GenActivePW", random.nextDouble()).build();
   
           try {
               client.fastPublish(request);
               System.out.println(" post measurepoint success...");
           } catch (Exception var3) {
               var3.printStackTrace();
           }
       }
   	
   } public static void postMeasurepoint() {
        Random random = new Random();
        System.out.println("start post measurepoint ...");
        MeasurepointPostRequest request = (MeasurepointPostRequest)MeasurepointPostRequest.builder().addMeasurePoint("INV.GenActivePW", random.nextDouble()).build();
        try {
            client.fastPublish(request);
            System.out.println(" post measurepoint success...");
        } catch (Exception var3) {
            var3.printStackTrace();
        }
    }
   ```

6. 使用`handleServiceInvocation()`方法响应云端的服务命令。

   ```java
    public static void handleServiceInvocation() {
        IMessageHandler<ServiceInvocationCommand, ServiceInvocationReply> handler = new IMessageHandler<ServiceInvocationCommand, ServiceInvocationReply>() {
            public ServiceInvocationReply onMessage(ServiceInvocationCommand request, List<String> argList) throws Exception {
                System.out.println("rcvn async serevice invocation command" + request + " topic " + argList);
                return (ServiceInvocationReply)ServiceInvocationReply.builder().addOutputData("execResult", 0).build();
            }
        };
        client.setArrivedMsgHandler(ServiceInvocationCommand.class, handler);
    }
   ```

SDK具体使用参考[SDK设备端连接](../howto/device/develop/using_java_sdk)。

## 步骤5：查看设备连接状态<checkconnection>

在EnOS控制台中选择 **设备管理**，在设备列表中，查看 **INV001** 设备的状态，确认设备处于 **在线** 状态。

.. image:: ../media/device_status.png

## 步骤6：查看设备数据<viewdata>

1. 在设备列表中，找到 **INV001** 设备，并点击 **操作** 列中的 **查看** 图标，进入 **设备详情** 页面。
2. 点击 **测点** 标签，找到测点 **INV.GenActivePW**，点击 **查看数据**，打开 **时序洞察** 页面。
3. 查看测点的最新数据。如果已为该测点配置存储策略，亦可在时序洞察页面生成该测点的历史数据图表。有关时序洞察的详细信息，参见[生成时序数据图表](/docs/data-asset/zh_CN/latest/howto/storage/generating_data_chart.html)。

## 步骤7：通过在线调试工具调试测点置数

1. 点击 **设备管理 > 产品管理**，点击所需调试设备所属的产品操作栏的 **查看**。

2. 在产品详情页，点击**在线调试**。

3. 选择所需调试的设备。

4. 从**调试功能**下拉菜单中选择所需调试的模型点及**方法**，在该示例中，选择**INV.GenActivePW**，**设置**，并点击**发送指令**。

   .. image:: media/debug_postmeasurepoint.png

测点置数成功后，在Java开发环境中你会收到以下回应：

```
AnswerableMessageBody{id='21', method='thing.service.measurepoint.set', version='1.0', params=IVN.GenActivePW=2.0}}
```

<!--end-->