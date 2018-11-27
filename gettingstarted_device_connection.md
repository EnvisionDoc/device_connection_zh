# 直连设备连接快速入门

该文章帮助你快速入门将直连设备预配至EnOS Cloud，从设备发送数据至EnOS Cloud，并从EnOS Cloud查看设备通信信息。

## 场景描述

接入场景参考[设备接入方案](connection_scenarios)当中提到的“场景2.2”。


## 任务描述

本示例以户用光伏逆变器接入为例进行说明，逆变器采集器出厂烧录逆变器设备三元组。逆变器上电、联网以后，基于设备三元组认证直连云端IoT Hub。整体流程如下图所示：

  ![](media/device_connection_task_description.png)

基于上述接入流程图，本示例主要有以下任务：
1. 创建设备模型
2. 创建产品
3. 注册设备
4. 通过设备SDK模拟设备发送数据
5. 查看设备通信状态
6. 查看设备数据


## 步骤1：创建设备模型

该步骤假设没有可以复用的设备模型，我们创建一个名称为***Inverter_Demo*，功能定义如下表的新的模型：

<table>
    <tr>
      <th>功能类型</th>
      <th>名称</th>   
      <th>标识符</th>   
      <th>数据类型</th>   
      <th>数据定义</th>   
    </tr>
    <tr>
      <td>属性</td>
      <td>逆变器类型</td>     
      <td>invType</td>
      <td>enum</td>  
      <td>{0:Central,1:String}</td>      
    </tr>
    <tr>
     <td>属性</td>
      <td>组件容量</td>
      <td>capacity</td>     
      <td>float</td>
      <td>kWp</td>      
    </tr>
    <tr>
      <td>测点</td>
      <td>有功功率</td>     
      <td>INV.GenActivePW</td>
      <td>float</td>  
      <td>kW</td>      
    </tr>
    <tr>
      <td>服务</td>
      <td>控制</td>     
      <td>INV.Control</td>
      <td>--</td>  
      <td>调用方式:异步调用</td>      
    </tr>
    <tr>
      <td>事件</td>
      <td>故障信息</td>     
      <td>Error</td>
      <td>--</td>  
      <td>事件类型:故障</td>      
    </tr>
</table>

创建该模型的步骤如下：

1. 在EnOS控制台中选择**接入管理 > 模型管理**。
2. 点击在页面右上方**创建模型**, 并在**创建模型**窗口提供以下配置信息：
  - **模型标识符**： Inverter_Demo
  - **模型名称**：Inverter_Demo
  - **模型名称(英文)**：Inverter_Demo
  - **分类**：无
  - **模型关系**：无
  - **模型模板**：无
  - **模型描述**：Inverter model for demo project

  ![](media/model_inverter.png)

3. 点击**确定**完成操作。
4. 点击**查看**，在模型详细信息界面中点击**功能定义**标签。
5. 点击**新增**，并在**添加功能**窗口提供以下配置信息：
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
    - **数据类型**：float
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

有关设备模型设置的详细信息，参见[创建模型](creating_model)。


## 步骤2：创建产品

在该步骤，我们创建一个名为**Inverter_Product**的产品，我们假设该产品型号的设备通过JSON格式上送数据且数据传输不使用CA证书加密。

1. 在EnOS控制台中选择**接入管理 > 产品管理**。
2. 点击在页面右上方**创建产品** ，在**创建产品**窗口提供以下配置信息。
  - **产品名称**：Inverter_Product
  - **节点类型**：设备
  - **设备模型**：Inverter_Demo
  - **数据格式**：Json
  - **证书双向认证**：禁用
  - **产品描述**：Inverter product for demo

  ![](media/create_product.png)

填写规范如下表所示：
<table>
    <tr>
  		<th>参数
  		</th>
  		<th>输入值
  		</th>      
  	</tr>
    <tr>
  		<td>产品名称
  		</td>
  		<td>产品名不可重复，支持中文、英文字母、数字和下划线
  		</td>      
  	</tr>
    <tr>
  		<td>节点类型
  		</td>
  		<td>选择设备
  		</td>
  	</tr>
    <tr>
  		<td>设备模型
  		</td>
  		<td>选择一个已定义好的模型。如果选择的模型还未定义功能，则可在产品创建后再设置。
  		</td>
  	</tr>
    <tr>
  		<td>数据格式
  		</td>
  		<td>JSON:
      透传：
  		</td>
  	</tr>
    <tr>
  		<td>产品描述
  		</td>
  		<td>产品名称
  		</td>
  	</tr>
  </table>
3. 点击**确定** 完成操作。

有关产品设置的详细信息，参见[创建产品](creating_products)。


## 步骤3：注册设备

在该步骤中，我们创建一个名称为**INV001**的设备，该设备属于在上一步骤中创建的**Inverter_Product**产品型号。

1. 在EnOS控制台中选择 **接入管理>设备管理**。
2. 点击在页面右上方 **添加设备** ，在弹出窗口配置如下信息：
  - **产品**：Inverter_Product
  - **Device Name**：INV001
  - **逆变器类型**：0:Central，表示集中式逆变器
  - **组件容量**：5.0
  - **Device Key**：选填，系统自动生产

  ![](media/register_device.png)

填写规范如下表所示：
<table>
  <tr>
		<th>参数
		</th>
		<th>描述
		</th>
	</tr>
  <tr>
		<td>产品</td>
		<td>设备所归属的产品。根据产品配置的不同，会出现相应的选项。
		</td>
	</tr>
  <tr>
		<td>DeviceName	</td>
		<td>设备名，在同一产品下设备名可重名。例如：INV001。
		</td>
	</tr>
  <tr>
		<td>Devicekey</td>
		<td>
    可选填。
    如果不填，则系统自动生成。
    如果自定义设置，需要保证在组织下唯一，并只支持大小写字母[a-z][A-Z]，数字[0-9]，横杠"-"，下划线"_ "。
    </td>
  </tr>
  <tr>
    <td>模型属性</td>
    <td>必填项属性在注册设备的时候就必须要填写，选填项可以在设备注册以后再补充。
    </td>
  </tr>
</table>


## 步骤4：SDK模拟设备发送数据

在该步骤中，我们通过设备端SDK模拟发送逆变器有功功率至云端。

1. 获取[设备端SDK](https://github.com/EnvisionIot/enos-mqtt-java-sdk)。更多信息，参考该SDK的GitHub readme文件。
2. 配置EnOS Cloud连接地址。
3. 将注册设备获取的设备三元组（`ProductKey`,`DeviceKey`,`DeviceSecret`）配置到sample连接程序当中。
4. 修改**postSubMeasurepoint**方法，配置发送数据测点名称，这里发送逆变器有功功率点，设置点名**INV.GenActivePW**，以及对应的点值。

SDK具体使用参考[SDK设备端连接](using_sdk)。

## 步骤5：查看设备连接状态

进入控制台，选择**接入管理>设备管理**，查看INV001设备的状态，确认设备处于在线状态。

  ![](media/device_status.png)


## 步骤6：查看设备数据

进入控制台，选择**接入管理>设备管理**，进入**设备详情**，打开**测点**tab页面，选择一个测点，点击**查看数据**，可以查看历史数据记录。