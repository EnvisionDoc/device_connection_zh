# 教程：使用设备模拟器

设备模拟器让你能够在不使用EnOS Java SDK的情况下，模拟一个在EnOS注册了的设备发送数据至EnOS，以便熟悉EnOS各项功能、或调试故障。使用Java SDK模拟设备接入EnOS的教程，参见[Connecting a Simulated Device into EnOS](../../../tutorial/connecting_device_simulated/index)。

本教程使用设备模拟器，模拟一个设备向EnOS上报测点数据，其上报的样本数据中加入了异常数据，会触发相应告警。同时配置存储策略、然后在 **时序数据管理 > 数据洞察** 中观察模拟的数据样本。

## 任务描述

本文模拟一个智能电流表，每一分钟上报一次测量的电流数据，如果其上报的实时电流超过60毫安，则EnOS发出“Current is above threshold”告警。

.. image:: ../../../media/simulated_ammeter.png

通过 **时序数据管理 > 数据洞察** 功能，可以看到该电流表的实时数据。

该教程各步骤的顺序及逻辑关系如下图所示，图中的步骤编号对应[步骤](#步骤)中的步骤编号：

.. image:: ../../../media/simulated_ammeter_flowchart.png

## 开始前准备

- 你需要有以下功能的权限，如果没有，请联系组织管理员添加，参见[策略，角色，与权限](/docs/iam/zh_CN/2.0.9/access_policy)：

    - 模型管理
    - 设备管理
    - 告警管理
    - 时序数据管理

## 步骤

### 步骤1：创建模型、产品，注册设备

1. 创建一个虚拟电流表的模型，其具体参数如下，创建模型的具体步骤参见[创建模型](../../model/creating_model)：

  .. csv-table:: 虚拟电流表模型
     :widths: auto
     :header: "字段", "取值"

     "模型标识符", "test.ammeter"
     "模型名称", "虚拟电流表"
     "分类", "无"
     "模型关系", "无"
     "模型模板", "无"
     "模型描述", "无"


2. 为该设备定义如下的要素：

  .. csv-table:: 虚拟电流表模型要素
     :widths: auto
     :header: "字段", "取值"

     "功能类型", "测点"
     "名称", "实时电流"
     "标识符", "current"
     "测点类型", "AI"
     "是否有质量位", "无"
     "数据类型", "double"
     "单位", "电流：毫安 | mA"
     "自定义标签", "无"
     "描述", "无"


3. 基于 **虚拟电流表** 模型，创建一个产品，具体参数如下，创建产品的具体方法，参见[创建产品（设备集合）](creating_product)：

  .. csv-table:: 虚拟电流表产品
     :widths: auto
     :header: "字段", "取值"

     "产品名称", "虚拟电流表产品"
     "节点类型", "设备"
     "模型", "虚拟电流表"
     "数据格式", "JSON"
     "证书双向认证", "禁用"
     "产品描述", "无"

4. 基于 **虚拟电流表产品** ，注册一个虚拟电流表设备，其参数如下，注册设备的具体方法，参见[注册设备](creating_device)：

  .. csv-table:: 虚拟电流表设备
     :widths: auto
     :header: "字段", "取值"

     "产品", "虚拟电流表设备"
     "Device Key", "simulatedAmmeter"
     "设备名称", "虚拟电流表设备"
     "时区/城市", "UTC+08:00"

### 步骤2：配置时序数据存储策略

1. 在 **时序数据管理 > 存储策略** 中，为 **虚拟电流表产品** 配置TSDB存储策略。

  1. 选择一个已有的策略分组，或者新建一个分组，点击 **修改分组** ，勾选 **test.ammeter** ，点击 **确认** ，将该模型纳入到该策略分组中。

  2. 点击 **AI原始数据** 右边的 |edit| ，勾选 **test.ammeter** ，将该模型下所有测点加入当前存储策略。
    
    由于之前配置的测点 **实时电流** 为AI类型数据，本教程也无需对其进行归一化处理，所以将测点加入 **AI原始数据** 存储策略中。

    .. |edit| image:: ../../../media/button_edit.png

### 步骤3：配置告警规则

1. 在 **告警管理** 中，创建一条如下的告警规则。创建告警规则，还需要创建相应的告警级别、类型、内容。创建这些告警要素的具体方法，参见[告警服务](../../alert/index.rst)：

  .. csv-table:: 告警规则配置
     :widths: auto
     :header: "字段", "取值"

     "规则编号", "ammeter_high_alert"
     "选择模型", "虚拟电流表"
     "测点", "实时电流"
     "触发条件", ">"
     "Value", "60"
     "告警内容", "Current is above threshold"
     "告警级别", "Warning"
     "延后告警", "0"
     "告警屏蔽", "禁用"
     "是否启用", "启用"

  该告警规则的含义是：当以 **虚拟电流表** 为模型创建的设备，其上报的 **实时电流** 测点值大于60（毫安）时，EnOS就会发出告警内容为 **Current is above threshold** ，级别为 **Warning** 的告警。

### 步骤4：使用设备模拟器

1. 在 **设备管理 > 设备模拟器** 中，为 **虚拟电流表设备** 添加模拟器，添加的具体步骤参见[使用设备模拟器](using_device_simulator)。

  .. image:: ../../../media/added_simulated_ammeter.png

2. 为 **虚拟电流表设备** 添加数据样本，具体方法参见[使用设备模拟器](using_device_simulator)。

  打开样本，里面的数据分成两列，**timeOfDay** 表示一天之内，从0:00：00到23:59:00的分钟级时刻，**current** 则表示每分钟模拟器上报的模拟的测点值。

  由于之前设置的告警中，我们设定当 **实时电流** 大于60毫安时告警触发。因此我们需要确保在样本中，至少有某一分钟的测点值大于60，这样才能触发 **Current is above threshold** 告警。

  一个比较好的做法是，在样本中比较靠前的时刻，将测点值设置为一个大于60的double类型的值。这样你无需等待过长时间，就能观察到告警的触发。如下图，本教程在第2分钟、第7分钟和第10分钟，设置了大于60的测点值：

  .. image:: ../../../media/simulator_data_sample.png

3. 启动设备模拟器，将结束时间设置为24小时之后。

  模拟器结束时间可自由设定，将结束时间设置得更晚，可以确保告警能够产生，同时TSDB中有足够量的数据可供观察。

## 结果

### 时序洞察数据报表

等待一段时间后，在 **时序数据管理 > 时序洞察** 中，选择 **虚拟电流表设备** ，可以观察到 **实时电流** 数据的分钟级的数据报表。

  .. image:: ../../../media/simulated_ammeter_tsdb_data.png

### 告警记录

在 **告警级别 > 告警记录** 中，查看 **虚拟电流表** 模型当天的历史告警，可以查看到数据样本中超过告警规则阈值的数据触发的相关告警。

  .. image:: ../../../media/simulated_ammeter_historical_alerts.png




