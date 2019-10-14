# 单元 1: 在EnOS控制台上注册个人电脑

在将PC连接到EnOS IoT中心之前，您需要在EnOS控制台上进行注册，包括定义计算机型号，创建计算机产品以及将PC注册为可以直接连接到EnOS Cloud的设备。

## 步骤 1: 定义电脑模型

模型是对连接到IoT平台的设备功能的抽象。该模型定义设备的功能，包括属性，测点，服务和事件。有关模型的更多信息，参见[物模型](/docs/device-connection/zh_CN/2.0.9/howto/model/model_overview.html).

此步骤需确认没有在EnOS上重复使用的计算机的模型。采取以下步骤来创建名为 **Computer** 的模型并定义所需的功能。

1. 登录至EnOS控制台，点击左侧导航栏中的 **模型**。

2. 点击页面右上方 **创建模型**，在 **创建模型** 页面配置下列信息：

   - **模型标识符**：Computer
   - **模型名称**：Computer
   - **分类**：NA
   - **模型关系** ：无
   - **模型模板**：无
   - **模型描述**：电脑模型

3. 点击 **确定** 完成操作。

   .. image:: media/create_model.png

4. 在创建模型列表，点击 **编辑** 进入刚创建模型的 **模型详情** 页，选择 **功能定义** 标签，点击 **新增**，在弹出的 **新增功能** 窗口中为产品属性设置一个或多个功能要素。

   - 定义以下属性：

     .. csv-table::
        :widths: auto

        "功能类型", "名称", "标识符", "数据类型", "数据长度", "描述"
        "属性", "系统", "系统", "string", "64 字符", "电脑系统信息"
        "属性", "模型", "模型", "string", "128 字符", "电脑模型信息"
        "属性", "cpu_core", "cpu_core", "int", "--", "电脑CPU核心数量"
        "属性", "mem_total", "mem_total", "double", "--", "电脑内存"

   - 定义以下测点：

     .. csv-table::
        :widths: auto

        "功能类型", "名称", "标识符", "测点类型", "数据类型", "描述"
        "测点", "cpu_used", "cpu_used", "AI", "double", "获取已用CPU数据的测点"
        "测点", "mem_used", "mem_used", "AI", "double", "获取已用内存数据的测点"
        "测点", "cpu_percent", "cpu_percent", "AI", "double", "获取CPU使用率的测点"
        "测点", "mem_percent", "mem_percent", "AI", "double", "获取内存使用率数据的测点"

   - 定义以下服务：

     .. csv-table::
        :widths: auto

        "功能类型", "名称", "标识符", "调用方式", "输入参数", "输出参数", "描述"
        "服务", "control", "control", "同步", "interval (int)", "result (enum; 0:success, 1:failure)", "控制计算机数据上传频率的服务"

   - 定义以下事件：

     .. csv-table::
        :widths: auto

        "功能类型", 名称", "标识符", "事件类型", "输出参数", "描述"
        "事件", "cpu_event", "cpu_event", "告警", "value (double); message (string)", "监控CPU负载的事件"

   请参见以下模型创建图示：

   .. image:: media/model_features.png


设备模型设置详情请见 [创建模型](/docs/device-connection/zh_CN/2.0.9/howto/model/creating_model.html).


## 步骤 2: 创建电脑产品

产品是具有相同功能的设备的集合。产品根据设备型号进一步定义了设备的通信规格。

在此步骤中，创建一个名为 **Computer** 的产品。 假设此产品型号的设备以JSON格式发送数据，并且未使用CA证书对数据传输进行加密。

1. 在EnOS控制台中选择 **设备管理 > 产品管理**。

2. 点击在页面右上方 **创建产品**，在 **创建产品** 页面配置下列信息。

   - **产品名称**: Computer
   - **节点类型**: Device
   - **设备模型**: Computer
   - **数据格式**: JSON
   - **证书双向认证**: Disabled
   - **产品描述**: Personal Computer

3. 点击 **确定** 来创建该产品。

   .. image:: media/create_product.png


设置产品详情请见：[创建产品](/docs/device-connection/en/2.0.9/howto/device/manage/creating_product.html).


## Step 3: 将PC注册为设备

设备是产品的实例。从产品创建设备后，它不仅继承模型的基本功能，而且还继承产品的通信功能（设备密钥和用于安全通信的设备证书）。

在此步骤中，创建一个名为 **PC_Win10** 的设备，该设备从属于上一步中创建的 **Computer** 产品。

1. 在EnOS控制台中选择 **设备管理 > 设备资产**。

2. 点击在页面右上方 **添加设备**，在 **添加设备** 页面配置下列信息。

   - **产品**: Computer
   - **设备名称**: PC_Win10
   - **时区/城市**: UTC+08:00
   - **使用夏令时**: 不勾选
   - **Device Key**: 选填(可由系统自动生成)
   - **system**: 填写电脑系统信息(for example, Win10)
   - **model**: 填写电脑模型信息(例如, E480)
   - **cpu_core**: 填写电脑CPU核心数量(例如, 4)
   - **mem_total**: 填写电脑总内存(例如, 8000000000)

   .. note:: 现在输入属性信息的估计值。待将PC设备连接到EnOS之后，EnOS将采集并更新实际属性。

3. 点击 **确定** 来创建该设备，如下图所示。

   .. image:: media/register_device.png   


设备注册详情参见：[注册设备](/docs/device-connection/zh_CN/2.0.9/howto/device/manage/creating_device.html).

完成PC的注册后，从设备列表中找到已注册的目标设备，点击该设备的 **查看** 进入设备详情页面，获得设备的`Product Key`, `Device Key` 和 `Device Secret`属性信息，这些属性信息将用于将PC连接到EnOS IoT中心。 请参见以下示例：


.. image:: media/device_properties.png   



## 下一单元

[为设备数据设置存储策略](configuring_storage_policy)
