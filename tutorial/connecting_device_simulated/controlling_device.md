#  Unit 4: Controlling Device State

After the battery device is connected into the EnOS Cloud, you can send commands to control the device state with the defined service. In this tutorial, use the *start_charging* service defined in the *battery* model to control the charging state of the battery device. Detailed steps are as follows:

1. Define the *start_charging* service for the *battery* model. See the following example:

   .. image:: media/service_parameters.png

   With the *start_charging* service, you can send instructions from the cloud to the battery device.

2. Declare the following main functions for simulating device data:

   ```java
   public static void main(String[] args) throws Exception {
       initWithCallback();
       handleServiceInvocation();
   }
   ```

3. Use the `handleServiceInvocation` function to process the service invocation:

   ```java
   public static void handleServiceInvocation() {
           IMessageHandler<ServiceInvocationCommand, ServiceInvocationReply> handler = new IMessageHandler<ServiceInvocationCommand, ServiceInvocationReply>() {
               public ServiceInvocationReply onMessage(ServiceInvocationCommand request, List<String> argList) throws Exception {
                   System.out.println("rcvn async service invocation command" + request + " topic " + argList);
                   return (ServiceInvocationReply)ServiceInvocationReply.builder().addOutputData("point1", 11).build();
               }
           };
           client.setArrivedMsgHandler(ServiceInvocationCommand.class, handler);
       }
   ```

4. Use the debugging function of the **Product** configuration to test sending instructions to the device.

   .. image:: media/debugging_device.png

5. Select the *start_charging* service from the **Debugging** field, input a value for the *result* parameter, and click **Run**. The instruction will be sent to the device.

   .. image:: media/service_instruction.png

6. The running program will receive the service invocation message. See the following example:

   ```
   rcvn async service invocation commandAnswerableMessageBody{id='2267850613537742848', method='thing.service.start_charging', version='1.0', params={result=1}} topic [OjupXK76, keyofbattery1, start_charging]
   ```


## Next Unit

[Monitoring the Device Performance with Alerts](monitoring_alarms)
