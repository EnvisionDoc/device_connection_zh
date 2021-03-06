# 创建产品（设备集合）

本文描述了如何创建一个产品以管理一批抽象于相同模型的设备。

## 开始前准备<beforestart>

- 你需要有设备管理操作权限，如果没有需联系组织管理员添加。有关EnOS内的用户权限，参见[策略，角色，与权限](/docs/iam/zh_CN/2.0.9/access_policy)。
- 该产品对应的模型已经被创建。有关如何创建，参见[创建模型](../../model/creating_model)。


## 步骤1：创建产品<createproduct>

1. 在EnOS控制台中选择 **设备管理 > 产品管理**。

2. 点击在页面右上方 **创建产品**，在 **创建产品** 页面配置下列信息。

   - **产品名称**：产品的名称，在同一组织内具有唯一性。
   - **节点类型**：定义该节点的类型。
     + 设备：没有需要连接的子设备。
     + 网关：需要连接子设备。
   - **设备模型**：产品是所采用的模型。
   - **数据格式**：从设备端发来的数据的格式。
     + JSON模式：格式必须满足模型中定义的属性生成的JSON交互格式。
     + 透传模式：透传模式下数据格式不受限，甚至允许二进制数据。但需要在该产品上创建一个自定义的数据解析脚本，用于编解码上下行的数据。
   - **证书双向认证**：该产品的认证方式。选择是否启用证书双向认证方式。
   - **产品描述**：该产品的描述。

3. 点击 **确定** 来创建该产品。

## 步骤2：（可选）添加标签<addtags>

标签描述了同类产品所具有的共性信息。

1. 从设备列表中找到目标设备并点击设备后 **查看**。

2. 在 **基础信息** 标签下的 **标签** 区域中点击 **编辑**。

3. 在弹出窗口中，点击 **创建标签**，输入新标签的键值对（key:value）。

4. 点击 **OK** 来保存标签。

## 结果<result>

当创建完Product后，可以获取一组凭证：Product Key（`productKey`）和Product Secret（`ProductSecret`）。这一对凭证唯一的标明此Product的身份，也是对此Product下进行设备注册以及Product本身信息修改的钥匙。产品密钥凭证将被用于设备的激活。更多信息，参考[基于密钥的单向认证](../../../learn/deviceconnection_authentication)。

## 后续操作<followup>

如果在创建产品时，**数据格式** 选择了 **透传模式**，则需要上传数据解析脚本，具体请参考[创建解析数据脚本](creating_data_parsing_script)。

## 相关信息<relatedinformation>

- [创建模型](../../model/creating_model)
- [创建设备](creating_device)
