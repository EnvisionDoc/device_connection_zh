# 设备信息上报 (透传)

如果使用透传模式，设备可上报原始数据（如二进制数据流），将对应的二进制流发送至EnOS Cloud。原始数据可以包含设备测点、属性、事件信息。EnOS Cloud会根据用户提交的数据解析脚本将二进制流转换成相应的标准数据格式。透传消息没有固定的消息体。


上行
- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/model/up_raw`

- 响应TOPIC：`/sys/{productKey}/{deviceKey}/thing/model/up_raw_reply`

有关配置数据解析脚本的更多信息，参见[创建数据解析脚本](../../../../howto/device/manage/creating_data_parsing_script) 。


