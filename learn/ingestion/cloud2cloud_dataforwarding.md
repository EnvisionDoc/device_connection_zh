# 第三方云数据转发至EnOS Cloud

## 场景描述

设备先接入到第三方云，然后通过第三方云将设备数据转发给EnOS Cloud。

## 整体方案概述

针对上述场景，可以将第三方云看成EnOS上的一个App，假定将其命名为`数据转发App`。这个App通过EnOS平台提供的`SA`账号，调用Restful接口，将测点数据上送至EnOS平台。整体流程如下图所示：

.. image:: ../../media/cloud2cloud.png

### 第一部分：EnOS Cloud配置

1. 预先在EnOS Cloud上创建好设备模型、产品、设备。
2. 提供给`数据转发App`使用的SA。

### 第二部分： 数据转发App抽象、转换和转发数据

1. `数据转发App`是一个部署在第三方云上的应用，将`SA`配置到`数据转发App`。
2. 以第三方云作为数据源，`数据转发App`读取第三方云数据源设备主数据信息，获得设备数据转发列表。
3. `数据转发App`提取设备数据，然后将数据转换成EnOS Cloud要求的格式。
4. `数据转发App`调用restful接口将数据发送至给EnOS Cloud。

### 第三部分：新设备管理

1. `数据转发App`监听第三方云数据源设备主数据信息，如果第三方云上新增了设备：
2. 调用Restful接口同步在EnOS Cloud上创建设备。
3. 将设备添加到数据转发列表。

## 如何实现

### 第一部分：EnOS Cloud设置

1. 创建模型参考：[创建模型](../../howto/model/creating_model)。
2. 创建产品参考：[创建产品](../../howto/device/manage/creating_product)。
3. 创建设备参考：[创建设备](../../howto/device/manage/creating_device)。
4. 创建应用参考：[管理应用](https://www.envisioniot.com/docs/app-development/zh_CN/latest/managing_apps.html)。创建应用以后，平台为应用分配`accessKey`和`secretKey`，凭借`accessKey`和`secretKey`可以调用EnOS API接口。

### 第二部分：数据转发App

1. 配置服务账户至应用。

2. 维护第三方云设备主数据与EnOS Cloud主数据映射。

   .. note:: 在第三方云上整理设备主数据id，可以将设备主数据id作为EnOS Cloud的deviceKey，并且每个设备必须关联其在EnOS Cloud上对应的product。

3. 测点数据上报主要涉及两个接口，获取设备信息：主要基于deviceKey获取assetId，对应接口getDeviceByDeviceKey。

4. 发送测点数据：对应接口uploadDeviceMeasurepoints。

5. 创建设备：对应接口registerDevices。

接口调用参考：[EnOS REST API 快速入门](https://www.envisioniot.com/docs/app-development/zh_CN/latest/gettingstarted_api.html)
assetId可以基于getDeviceByDeviceKey接口动态查询获得，整体映射表如下表所示：

.. list-table::
   :widths: auto

   * - 3rd party Cloud DeviceID
     - EnOS ProductKey
     - EnOS AssetId
   * - SN000001
     - XmrETyvF
     - Ji7bipvy
   * - SN000002
     - EtHYdYP6
     - 3kxJLXgg

<!--end-->
