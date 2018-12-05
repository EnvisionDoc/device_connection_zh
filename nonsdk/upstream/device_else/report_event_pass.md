# 设备事件上报 (透传)

如果使用透传模式，设备可上报原始数据如二进制数据流，设备端会将对应的二进制流发送至EnOS Cloud，EnOS Cloud会根据用户提交的脚本将二进制流转换成相应的标准数据格式。透传消息没有固定的消息体。
请求和响应的格式可参考设备事件上报 (非透传)。

上行
- Request TOPIC: /sys/{productKey}/{deviceKey}/thing/model/up_raw

- Reply TOPIC：/sys/{productKey}/{deviceKey}/thing/model/up_raw_reply
