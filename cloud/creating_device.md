# 创建设备

设备是模型的实例。设备归属于某个产品。平台为设备颁发组织(Organization)内唯一的证书DeviceKey。设备可以直接连接平台，也可以作为子设备通过网关连接物联网平台。该文章描述了如何创建一个设备。

## 开始前准备<beforestart>

该设备所属的产品已经被创建。有关如何创建，参见[创建产品](creating_product)。

## 步骤1：创建设备<createdevice>

1. 在EnOS控制台中选择 **设备管理** 。

2. 点击在页面右上方 **添加设备** ，在弹窗页面上配置下列信息。

   - **产品**：选择设备所属的产品。
     选定产品后，继续配置该产品对应的模板中设置的属性功能，分为必填和选填。
   - **Device Name**：该设备的名字，同组织下可同名。
   - **Device Key**：产品在同组织下的唯一识别码。如果不填，则由系统自动生成。如果自定义，该字段支持英文、数字、划线（ - ）和下划线（ _ )。

3. 点击 **确定** 来创建该设备。

## 步骤2：（可选）添加标签<addtag>

标签描述了该设备所有实例所具有的共性信息。

1. 从设备列表中找到目标设备并点击设备后 **查看**。

2. 在 **基础信息** 标签下的 **标签** 区域中点击 **编辑**。

3. 在弹出窗口中，点击 **创建标签**，输入新标签的键值对（key:value）。

4. 点击 **OK** 来保存标签。

##结果<result>

当创建完设备后，可以获取一组凭证：Device Name（`deviceName`）和Device Secret（`deviceSecret`）。EnOS Cloud为设备颁发的设备秘钥凭证将被用于设备的激活。更多信息，参考[基于密钥的单向认证](../secretbased_authentication)。

## 后续操作<followup>

新创建的设备在未上线的情况显示为未激活。如需激活设备，你需要通过设备端SDK发起连接。更多信息，参考[EnOS SDK 快速入门](https://docs.envisioniot.com/docs/app-development/zh_CN/latest/gettingstarted_sdk.html)

## 相关信息<information>

- [创建模型](../model/creating_model)
- [创建产品](creating_product)
