# 发送命令到设备

该文章介绍了如何处理从EnOS云端发送到设备的下游命令。在设备端，可在`onMessage`回调函数中配置响应信息，这样SDK便可响应云端的消息。你可以配置错误码和错误消息，用以向设备返回指令报错。对于此类自定义错误代码和错误消息，你可以选择使用2000或更大的数字。

在以下代码示例中，会对设置测量点和禁用的设备进行监控。

```java
 client.setArrivedMsgHandler(MeasurepointSetCommand.class, (MeasurepointSetCommand command, List<String> argList)->{
    boolean success = true;
    if(success){
        return MeasurepointSetReply.builder().build();
    }
    else {
        return MeasurepointSetReply.builder()
                .setCode(2000)
                .setMessage("handle the measurepoint set command failed")
                .build();
    }
});

client.setArrivedMsgHandler(SubDeviceDisableCommand.class, (SubDeviceDisableCommand command, List<String> argList)->{
    //如果你不想回复该消息，只需返回null即可
    System.out.println(command);
    return null;
});
```


.. note:: 如果用户响应了一个空回复，设备端在执行回调方法后将不会作出响应。如果用户响应了一个有效回复，SDK将会对该回复进行序列化处理并自动将其发送到云端。如果成功执行了回调方法，用户便可将响应代码设置为200或非200。如果执行失败，用户可以将自定义错误代码设置为2000或更大的数字，然后发送到云端。

<!--结束-->
