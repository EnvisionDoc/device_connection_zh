# 教程概述

## 应用场景

个人计算机是将设备连接到EnOS的最佳选择之一。使用开源插件，您可以采集PC的系统和硬件数据，并将采集的数据上传到EnOS Cloud进行进一步的处理和分析，从而可以监视CPU负载和内存使用情况。此外，您还可以从EnOS Cloud发送命令以控制数据采集间隔。

该场景如下图所示

.. image:: media/scenario_connect_pc.png

本教程将引导您在EnOS Cloud上建模以及将你的个人电脑注册至EnOS，采集PC系统和硬件数据，使用采集的数据更新PC的属性，将CPU和内存使用情况数据上传到EnOS Cloud，以及从EnOS Cloud发送命令以控制数据采集间隔。

在本教程中，您将：

- 为计算机定义模型并将PC注册为EnOS控制台上的设备
- 为PC系统数据配置TSDB存储策略
- 使用EnOS Java SDK for MQTT开发程序以将PC连接到EnOS
- 运行程序以提取PC数据并将数据上传到EnOS Cloud
- 监视PC的CPU负载
- 从EnOS Cloud发送命令以控制数据摄取间隔
- 开发用于计算PC内存使用率的流数据处理作业


### [开始 >](registering_device)

## 前提条件

- 您已经在EnOS注册了个人帐户或企业帐户，以访问EnOS控制台。
- 确保您的帐户拥有对模型服务、设备连接和管理服务以及数据资产管理服务的完全访问权限。

## 教程单元

本教程包括以下单元：

> [Unit 1. 在EnOS控制台上注册个人电脑](registering_device)
>
> 20 分钟

> [Unit 2. 为设备数据设置存储策略](configuring_storage_policy)
>
> 20 分钟

> [Unit 3. 将个人电脑连接至EnOS并提取数据](connecting_device)
>
> 30 分钟

> [Unit 4. 监测CPU负载](monitoring_cpu_load)
>
> 10 分钟

> [Unit 5. 控制数据上传间隔](controlling_upload_interval)
>
> 10 分钟

> [Unit 6. 计算内存使用百分比](calculating_memory_usage)
>
> 20 分钟
