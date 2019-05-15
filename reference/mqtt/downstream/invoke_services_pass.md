# 设备接收信息 (透传）

如果使用透传模式，设备可上报原始数据如二进制数据流，设备端会将对应的二进制流发送至EnOS Cloud，EnOS Cloud会根据用户提交的脚本将二进制流转换成相应的标准数据格式。透传消息没有固定的消息体,需要用户使用数据解析脚本自行定义。

下行

- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/model/down_raw`

- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/model/down_raw_reply`

有关配置数据解析脚本的更多信息，参见[创建数据解析脚本](../../../../howto/device/manage/creating_data_parsing_script)。