# NTP服务

你可以使用以下示例代码运用NTP服务。

```java
    public static void main(String[] args) {
        MqttClient client = new MqttClient(new FileProfile());
        System.out.println("local :"+ System.currentTimeMillis());
        System.out.println("fix :" + (client.getExtServiceFactory().getNtpService().getFixedTimestamp()));
        System.out.println("fix :" + (client.getExtServiceFactory().getNtpService().getFixedTimestamp(System.currentTimeMillis())));
        //使用NTP服务事件张贴测量点数据
        MeasurepointPostRequest request = MeasurepointPostRequest.builder()
                        .addMeasurePoint("point1", random.nextInt(100))
                        .setTimestamp(client.getExtServiceFactory().getNtpService().getFixedTimestamp())
                        .build();
        try {
            MeasurepointPostResponse rsp = client.publish(request);
            System.out.println("-->" + rsp);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
```
