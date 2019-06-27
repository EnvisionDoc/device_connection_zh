#  Unit 3: Simulating Posting Device Measuring Point Data

After the battery device is connected into the EnOS Cloud, you can simulate the temperature data of the battery using EnOS Device SDK and upload the simulated data to EnOS Cloud. Detailed steps are as follows:

1. Declare the following main functions for simulating device data:

   ```java
   public static void main(String[] args) throws Exception {
       initWithCallback();
       alwaysPostMeasurepoint();
   }
   ```

2. Use the `alwayspostMeasurepoint` function to simulate posting random temperature data of the battery repeatedly:

   ```
       public static void alwaysPostMeasurepoint() throws Exception {
           while(true) {
               long ts = System.currentTimeMillis();
               Random random = new Random();
               System.out.println("start post measurepoint ...");

               MeasurepointPostRequest request = MeasurepointPostRequest.builder().addMeasurePoint("temperature", random.nextDouble()).build();

               try {
                   client.fastPublish(request);
                   System.out.println(" post measurepoint success...");
               } catch (Exception var3) {
                   var3.printStackTrace();
               }
               System.out.println(client.isConnected() + " post  cost " + (System.currentTimeMillis() - ts) + " millis");
               Thread.sleep(10000L);
           }
       }
   ```

   If you want to simulate data of other measuring points, define the measuring point name in the `addMeasurePoint` function.

3. Check the running result of the program. The program will simulate random temperature data of the battery by the specified time interval in milliseconds and upload the data to EnOS Cloud.

4. Open the **Device Details** page of the *battery1* device, click **Measuring Points**, view the simulated temperature data of the battery.

   .. image:: media/view_data.png

5. Click **View data**, open the **Data Insights** page, and view the latest data of the **temperature** measuring point on the Data Insights page.

## Next Unit

[Controlling the Device Status](controlling_device)
