# 数据格式

设备按照模型中定义的属性、测点、事件、服务与云端进行数据传输。EnOS支持数据以EnOS标准JSON数据格式和自定义格式。

## EnOS标准数据格式

通常情况下，我们推荐设备以EnOS标准JSON数据格式进行数据传输。

该示例为向云端上报数据的标准格式参考：
```
{
        "id": "123",
        "version": "1.0",
        "params": {
                "measurepoints": {
                        "Power": {
                                "value": 1.0,
                                "quality": 9
                        },
                        "temp": 1.02,
                        "branchCurr": [
                                "1.02", "2.02", "7.93"
                        ]
                },
                "time": 123456
        },
        "method": "thing.measurepoint.post"
}
```

该示例为云端向设备下发数据的标准格式参考：
```
{
        "id": "123",
        "version": "1.0",
        "params": {
                "temperature": 30.5
        },
        "method": "thing.service.measurepoint.set"
}
```
在以上示例中：
- `id`是message ID
- `version`是协议版本
- `params`包裹的JSON数组为传输的数据
- `method`为请求方法

EnOS设备协议定义了数据以何种格式通过哪个topic与云端进行数据传输，详细信息，参考[设备协议](../../reference/mqtt/index).

## 自定义数据格式

由于低配置且资源受限或者对网络流量有要求的设备，不适合直接构造JSON数据和云端通信。在该场景下，你可以将数据透传到云端，由云端运行解析脚本将透传的数据转换成EnOS定义的JSON格式数据。云端向设备发送控制指令时，也可通过脚本将EnOS定义的JSON格式数据转换为设备能够理解的二进制数据进行下发。

对于使用透传/自定义格式传输数据的设备，你可以在EnOS Cloud编写脚本，解析设备数据。解析脚本支持使用JavaScript进行开发。

EnOS Cloud为开发者提供了以下功能:

1. 脚本在线编辑器，支持 JavaScript 语言，对语法进行静态检查；
2. 支持对脚本的模拟数据调试，可输入上下行数据进行模拟转换，查看脚本运行结果。
3. 提交脚本到运行环境，在设备上下行时进行调用。

获取更多信息，请参见[创建数据解析脚本](../../howto/device/manage/creating_data_parsing_script).
