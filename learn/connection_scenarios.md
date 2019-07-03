# 设备接入

通常根据设备的硬件能力及对设备接入的安全性要求选择接入方案。

在被接入物联网之前，首先对象要具有被连接的能力，换言之，至少应满足以下要求：

- 具有联网能力
- 支持烧录固件并运行连接程序

现实世界中的设备按照以上属性主要分成两类：

- **智能设备**：- 支持烧录固件，并可通过Wi-Fi、GPRS、3G或4G信号直连到IoT平台。智能设备可以直接接入到IoT Hub并与之通信，以完成认证和数据传输。
- **非智能设备**：- 不支持烧录固件，也不具备Wi-Fi、3G或4G联网的能力。该场景下，需要通过Edge网关来采集设备的数据，设备经由Edge网关连接至IoT平台。Edge需要支持固件烧录和联网。通过这种解决方案连接的设备又称作_sub device_。网关用作子设备的代理，以帮助它们完成认证、登录和数据传输等操作。

.. image:: ../media/device_types.png

## 智能设备接入<smartdeviceconnection>

此类设备可直接与云端连接。一些常见设备包括：

- 带智能采集棒的设备，例如户用逆变器、户用储能电池。
- 智能家居设备，例如监控摄像头、智能温湿度计。

## 非智能设备接入<nonsmartdeviceconnection>
此类设备需要通过网关代理才能与云端连接。一些常见设备包括：

- 分布式逆变器：网关直采多台逆变器的数据，并将数据发送至云端。

  .. image:: ../media/inverter_gateway.png

- SCADA：SCADA与风机直连，并采集风机数据；网关与SCADA连接，并采集SCADA数据，然后将数据发送至云端。

  .. image:: ../media/turbine_scada_gateway.png

## 安全认证选项<authentication>

EnOS IoT Hub支持的安全认证方式有两种：

- 基于密钥的认证：单向认证，安全性相对较弱，为系统默认认证方式。
- 基于证书的认证：双向认证，安全性高，需要用户主动开启。

有关设备接入安全认证机制的更多信息，参见[设备认证机制](deviceconnection_authentication).

## 场景和消息流<messageflow>

.. list-table::
   :widths: auto

   * - 场景编号
     - 连接方式
     - 激活方式
     - 是否使用SA
   * - 1.1
     - 直连
     - 使用设备密钥的静态激活
     - 否
   * - 1.2
     - 直连
     - 使用产品密钥的动态激活
     - 否
   * - 1.3
     - 直连
     - 使用设备密钥的静态激活
     - 是
   * - 2.1
     - 通过网关连接
     - 使用设备密钥的静态激活
     - 否
   * - 2.2
     - 通过网关连接
     - 使用设备密钥的静态激活
     - 是

下列章节描述了不同接入方式和激活方式选择方案的消息流。

### 场景1.1：单个预注册智能设备<scenario1_1>

下图所示为接入场景1.1的消息流。

.. image:: ../media/connection_scenario_1.1.png

.. note:: 该场景要求设备在出厂时已烧录在云端注册设备后得到的设备三元组。

由于在制造时需要烧录自身的三元组，因此该场景安全性较高，但是可操作性较低。

### 场景1.2：批量预注册智能设备<scenario1_2>

下图所示为接入场景1.2的消息流。

.. image:: ../media/connection_scenario_1.2.png

.. note:: 设备已在云端注册；设备烧录产品证书，且启用了动态激活。

该场景主要步骤如下：

1. 设备注册可以与设备厂商的设备管理系统进行集成。每发货一批次设备，客户的设备管理系统可以通过调用REST API批量注册设备。

2. 每个设备出厂批量烧录相同的产品证书（即`productKey`和`productSecret`）及以设备序列号作为`deviceKey`。

3. 当设备发往现场、上电、联网以后可通过`productKey`，`productSecret`和`deviceKey`动态获取`deviceSecret`。

4. 设备通过三元组，即`productKey`，`deviceKey`，和`deviceSecret`完成设备激活与上线，并开始传输数据。

场景1.2解决了场景1.1中烧录的可操作性较低的问题，但是因为设备出场烧录相同的产品证书，因此该场景的安全性低于场景1.1。

### 场景1.3：动态注册非智能设备<scenario1_3>

接入的设备未注册；设备通过可插拔数据采集棒动态注册。

下图所示为接入场景1.3的消息流。

.. image:: ../media/connection_scenario_1.3.png

以户用光伏逆变器为例进行说明。

户用光伏逆变器不支持烧录固件。该场景可视为场景1.1的延伸，在该场景中，需要使用采集棒进行数据采集并转发至云端。考虑采集棒只采集一台逆变器的数据，因此可以将逆变器和采集棒视为一个整体的智能设备，而采集棒支持烧录固件，所以可以将逆变器和采集棒作为一个可支持烧录固件的整体智能设备。  

1. 在云端创建逆变器产品（在客户的OU下创建，而非在开发者OU下创建）。

2. 采集棒开发者在开发者OU下创建采集棒应用，并获得采集棒应用的SA：`accesskey`和`accesssecret`。

3. 采集棒开发者对采集棒进行出厂配置，并在其中烧录以下凭证信息：

   - 烧录采集棒应用的SA
   - 烧录逆变器的`productKey`
   - `productKey`所属的`orgId`

4. 物联网实施人员进行现场施工安装，将采集棒安装在逆变器上，将设备上电和联网。设备联网后将进行下列动作：

   - 采集棒采集逆变器序列号，将序列号作为`deviceKey`，凭借SA调用REST API接口，通过`productKey`、`deviceKey`(序列号)、`orgId`动态创建设备，并获取设备的`deviceSecret`。
   - 采集棒记录`deviceSecret`，自动烧录到设备的固件当中。
   - 采集棒采集逆变器数据，使用`productKey`、`deviceKey`、`deviceSecret`连接云端。鉴权通过后，设备上线，然后开始发送遥测数据。

### 场景2.1：预注册子设备<scenario2_1>

下图所示为接入场景2.1的消息流。

.. image:: ../media/connection_scenario_2.1.png

场景2.1中，你需要事先在云端注册子设备，获取子设备三元组信息，并提前将子设备三元组信息烧录到Edge中。

在EnOS Edge配置中心配置设备连接时，需要将接入设备与预先烧录的子设备三元组绑定。

场景2.1的配置复杂度要大于场景2.2，场景2.2的灵活性更好，但是场景2.1的安全性更高。从便捷的角度，可以考虑使用场景2.2。

### 场景2.2：动态注册子设备<scenario2_2>


场景2.2的原理与2.1类似，场景2.2的灵活性更好，在场景2.2中，Edge中烧录了SA，具备调用EnOS API的权限，所以可以通过API创建子设备。

下图所示为接入场景2.2的消息流。

.. image:: ../media/connection_scenario_2.2.png

1. Edge开发者通过EnOS Console在EnOS Cloud注册一个Edge应用，并获得该应用的服务账号（SA），即`accessKey`和`accessSecret`。

2. 物联网实施人员登录到EnOS控制台，在客户组织中进行如下配置：

   - 创建Edge产品，并注册Edge设备实例，来获得Edge设备三元组。
   - 为待接入Edge的子设备创建产品，获得`productkey`。

3. 在Edge制造阶段，需要烧录以下凭据信息：

   - Edge应用的SA，将用于获得调用EnOS API的权限。
   - 由EnOS Cloud颁发的Edge设备三元组。
   - 待接入Edge的子设备所属的产品`productkey`，以及设备所属组织的标识，即`orgId`。

4. EnOS Cloud对Edge调用Restful接口进行如下鉴权：

   - Edge携带SA来获得调用EnOS API的权限。如果服务账号不对，则Edge将无法调用EnOS API，鉴权将失败。
   - EnOS Cloud基于IAM中定义的访问权限，校验Edge连接请求中携带的`orgId`和`SA`参数，并验证`orgId`所对应的组织是否注册了Edge应用。如果没有注册Edge应用，鉴权将失败。
   - EnOS Cloud校验请求参数`orgId`与`productkey`的归属关系。如果`productkey`对应的产品不属于`orgId`对应的组织，校验将不通过。

5. EnOS Cloud对Edge设备进行身份认证。

   - Edge默认启用基于密钥的单向认证。Edge携带三元组连接云端，其中云端会对Edge三元组进行认证，认证通过后，Edge设备将被允许登录。
   - Edge的首次登录会同时激活Edge设备，设备状态将从 **未激活** 更新为 **在线**。

6. 如果启用了基于证书的双向认证，证书的生成与分发过程如下（以EnOS Edge为例）：

   - EnOS Edge向EnOS IoT Hub发起证书请求，请求中携带证书请求文件（CSR文件）。
   - EnOS IoT Hub将请求转发给EnOS Certificate Service。
   - Certificate Service产生证书，并返回给IoT Hub。
   - IoT Hub记录下Edge关联的证书，并将Edge证书返回给Edge。

7. 物联网实施人员配置需要通过Edge接入的子设备（如逆变器、风机、储能设备等）连接。子设备注册有以下两种方式：

   - 设备注册：在EnOS Edge配置中心直接创建要接入的子设备，配置中心调用IoT Hub的REST API，然后在EnOS Cloud中创建设备。
   - 静态注册：在EnOS控制台中创建要接入的子设备，然后在Edge配置中心进行绑定。由Edge代理子设备连接至EnOS Cloud。

8. 设备数据传输

   - Edge与IoT Hub直接连接，子设备由Edge代理连接至EnOS IoT Hub。
   - Edge与与IoT Hub之间的数据传输，使用MQTT协议。
   - 如果启用基于证书的双向认证机制，Edge与IoT Hub之间的数据传输内容将被证书加密。
