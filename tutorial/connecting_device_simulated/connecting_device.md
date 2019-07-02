# Unit 2: Connecting Device into EnOS Cloud

In this step, use the EnOS Device SDK for Java to develop a program that simulates the battery device to connect into EnOS.

## Setting up Development Environment

EnOS Device SDK for MQTT for Java requires Java SE 8 and Maven 3. Take the following steps to set up your development environment:

1. Install JDK, which can be downloaded at <https://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html>.

2. Install Maven, which can be downloaded at <http://maven.apache.org/download.cgi>.

3. Install a development environment, such as IntelliJ IDEA, which can be downloaded at <https://www.jetbrains.com/idea/download/>.

In this tutorial, we'll use IntelliJ IDEA as an example.

## Installing EnOS Device SDK for MQTT for Java

The **SDK Center** on the EnOS Console lists all the EnOS SDKs with links to GitHub and Maven repositories. Take the following steps to install EnOS Device SDK for MQTT for Java:

1. Open the Maven repository of the SDK at https://mvnrepository.com/artifact/com.envisioniot/enos-mqtt/2.1.2.

2. Copy the maven dependency information of the SDK.

3. Open IntelliJ IDEA, include the maven dependency by adding the following code snippet in `pom.xml`. See the following example.

   ```
   <dependency>
       <groupId>com.envisioniot</groupId>
       <artifactId>enos-mqtt</artifactId>
       <version>2.1.2</version>
   </dependency>
   ```

Optionally, you can download the source code of the EnOS Device SDK from [GitHub](https://github.com/EnvisionIot/enos-mqtt-java-sdk) and install it in your development environment.

## Programming for Device Connection

After the EnOS Device SDK for MQTT for Java is installed, take the following steps to connect the battery device into EnOS Cloud (you can also refer to the `SimpleSendReceive.java` code sample available from `<SDK_Path>\src\main\java\com\envisioniot\enos\iot_mqtt_sdk\sample`):

1. Declare the public variables that will be used in the program, see the following example:

   ```java
   public static final String uri = "tcp://{host}:{port}";
   public static final String productKey = "product_key";
   public static final String deviceKey = "customized_key";
   public static final String deviceSecret = "device_secret";
   private static MqttClient client;
   ```

   - The `host` and `port` of the server vary with the cloud region and instance. For private cloud instances, contact your Envision project manager or support representative to get the host and port information.
   - The `productKey`, `deviceKey`, and `deviceSecret` are generated when you register the device in [Unit 1](registering_device).

3. Declare the main function `initWithCallback()` for initializing device connection. See the following example:

   ```
   public static void main(String[] args) throws Exception {
       initWithCallback();
   }
   ```

4. Use the `initWithCallback` function to connect the battery device into EnOS Cloud:

   ```java
   public static void initWithCallback() {
       System.out.println("start connect with callback ... ");

       try {
           client = new MqttClient(uri, productKey, deviceKey, deviceSecret);
           client.getProfile().setConnectionTimeout(60).setAutoReconnect(false);
           client.connect(new IConnectCallback() {
               public void onConnectSuccess() {
                   SimpleSendReceive.subDeviceLogin();
                   System.out.println("connect success");
               }

               public void onConnectLost() {
                   System.out.println("onConnectLost");
               }

               public void onConnectFailed(int reasonCode) {
                   System.out.println("onConnectFailed : " + reasonCode);
               }
           });
       } catch (EnvisionException var1) {
       }

       System.out.println("connect result :" + client.isConnected());
   }
   ```

5. Check the running result of the program. The program will return the following result if the device connection is successful:

   ```
   start connect with callback ...
   connect result :true
   ```

6. Check the status change of the battery device in the **Device List** on the EnOS Console. The status of the device will change from `Inactive` to `Online`.

   .. image:: media/device_state.png

## Next Unit

[Simulating the Device Measuring Point Data](simulating_data)
