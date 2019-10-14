# 单元 2: 为设备数据设置存储策略

EnOS TSDB可帮助你快速存储和读取重要且访问频次较高的业务数据。数据按类别（数据类型和存储时间）存储在TSDB中，从而降低了数据存储成本并提高了数据读取效率。

在将PC的CPU和内存使用率数据上传到EnOS Cloud之前，需要为上传的数据配置数据存储策略。否则，默认情况下，上传的数据将不会存储在EnOS TSDB中。

在本单元中，为创建计算机模型时定义的以下测点配置存储策略。


.. list-table::

   * - 测点
     - 存储类型
     - 描述
   * - cpu_used
     - AI Raw Data
     - When the CPU usage data of the PC is uploaded, store the raw data of the *cpu_used* measuring point in TSDB.
   * - mem_used
     - AI Raw Data
     - When the memory usage data of the PC is uploaded, store the raw data of the *mem_used* measuring point in TSDB.
   * - cpu_percent
     - AI Raw Data
     - When the CPU usage percentage data of the PC is calculated by the stream processing job, store the calculated data in TSDB.
   * - mem_percent
     - AI Raw Data
     - When the memory usage percentage data of the PC is calculated by the stream processing job, store the calculated data in TSDB.


存储类型详情请见 [Configuring TSDB Storage](/docs/data-asset/zh_CN/2.0.9/configuring_tsdb_storage.html).


## 步骤 1. 创建TSDB存储策略分组

在配置TSDB数据存储策略之前，需要先创建存储策略分组。

1. 登录EnOS控制台，选择 **时序数据管理 > 存储策略**。
2. 点击 **创建策略分组**，可进入创建策略分组配置页面进行存储策略分组的基本配置：
   - **分组名**：输入存储策略分组的名称。支持中英文、下划线、中横线。
   - **分组模型**：搜索并选择命名为 *Computer* 的模型，关联至存储策略分组。
3. 点击 **OK** ，保存策略分组配置。


## 步骤 2. 配置数据存储策略

After the storage group is created, you can see all the TSDB storage policy options listed under the storage group tab. Configure **AI Raw Data** storage policy for the *cpu_used*, *mem_used*, *cpu_percent*, and *mem_percent* measuring points by taking the following steps:

1. Move the cursor on the **AI Raw Data** storage type and click the **Edit** icon to open the **Edit Storage Policy** page.

2. From the **Storage Time** drop down list, select the storage time for the data (for example, 3 months).

3. Select the *Computer* model and the measuring points.

4. Click **OK** to save the storage policy. See the following example.

   .. image:: media/storage_policy_config.png

With the storage policy configured, ingested data and calculated data of the PC will be stored as **AI Raw Data** storage type.

## 下一单元

[将个人电脑连接至EnOS并提取数据](connecting_device)

<!--end-->
