# 告警管理快速入门
<!--
The short description should be a single, concise paragraph that contains one or two sentences and no more than 50 words.
Briefly mention what the user's learning goal is and include the following SEO keywords in the title short description: EnOS, ServiceName, tutorial.
-->

该文章帮助你使用设备告警服务管理基于设备实时数据的告警。

**开始前准备**

- 了解资产告警的主要概念及信息从测点采集至触发告警的流程，参考[资产告警概述](alert_overview)。
- 将设备接入至EnOS平台，过程如下：
  1. 完成设备建模
  2. 完成设备在EnOS云端的身份注册
  3. 完成设备端开发以使得设备可以通过EnOS的标准协议连接及发送数据，参考[设备端开发](../device/devlop/index)

    该准备的过程，可参考[将智能设备连接至EnOS](../../quickstart/gettingstarted_device_connection).

**步骤1. 配置告警级别**

根据事件的严重程度和处理优先级，对告警进行级别分类，以便正确处理告警。详见[创建告警级别](create_alert_severity)。

**步骤2. 配置告警类型**

为了解资产的状态或者区分触发告警的原因，对告警进行类型分类，以便更好地监控资产状态。告警类型还可包含子类型。详见[创建告警类型](create_alert_type)。

**步骤3. 配置告警内容**

定义告警的内容，包含对事件的详细描述，触发事件的原因，以及处理方法。定义告警内容时还需要选择告警的类型。详见[创建告警内容](create_alert_content).

**步骤4. 配置告警规则**

定义触发告警的规则，包括告警作用的设备模型和范围、触发告警的详细条件、以及告警内容和级别。详见[创建告警规则](create_alert_rule)。
