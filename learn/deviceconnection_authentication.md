# 安全认证机制

为保证设备与传输的安全，设备接入EnOS™ IoT Hub之前，需要经过鉴权。EnOS支持 **基于密钥的单向认证** 和 **基于证书的双向认证** 两种认证模式。

## 基于密钥的单向认证<secretbase>

讨论基于密钥的单向认证机制之前，你需要了解以下概念：

- **产品密钥证书**：`ProductKey`和`ProductSecret`。

  - `ProductKey`：是EnOS为产品颁发的全局唯一标识。
  - `ProductSecret`：由EnOS颁发的产品密钥，与`ProductKey`成对出现。常用于设备的动态激活，在设备初次连接EnOS Cloud时使用`ProductKey`、`ProductSecret`和`DeviceKey`可从云端获取`DeviceSecret`用于设备激活。

- **设备密钥证书**：`DeviceKey`和`DeviceSecret`。

  - `DeviceKey`:在注册设备时，自定义或系统自动生成的设备标识，具备OU内的唯一性。
  - `DeviceSecret`:由EnOS颁发的设备密钥，和`DeviceKey`成对出现。

- **设备三元组**：包含`ProductKey`、`DeviceKey`和`DeviceSecret`三要素，主要用于**基于密钥的认证**。

基于密钥的认证机制为系统默认强制执行。

### 基于密钥的单向认证概念<concepts>

基于密钥的认证是指基于设备三元组，即`ProductKey`、`DeviceKey`和`DeviceSecret`，对设备进行鉴权。

设备与EnOS IoT Hub连接主要涉及以下操作及状态：

- **注册**：即在云端创建一个设备，可以在基于网络的EnOS控制台中创建，也可以通过调用REST API来创建。

- **登录**：设备上送数据之前，首先必须成功登录，然后才可以发送数据。设备登录需要使用设备三元组鉴权。

- **激活**：设备首次登录成功后会被激活，将设备从**未激活**状态更新为**已激活**状态。**已激活** 状态包含 **在线** 和 **离线** 两个子状态，在EnOS控制台中会使用**在线**和**离线**两个子状态来代替显示 **已激活** 状态。

### 设备激活模式和设备状态<deviceactivation>

在EnOS平台初次创建的设备是默认处于启用但未激活的状态，等待被激活。设备激活分为 *动态激活* 和 *静态激活* 。

- **动态激活**：动态激活过程如下：

  1. 设备首次尝试连接时携带`ProductKey`，`ProductSecret`，`DeviceKey`来请求激活，鉴权通过以后，返回`DeviceSecret`给设备。

  2. 设备通过`ProductKey`，`DeviceKey`，`DeviceSecret`来尝试登录。

  3. 设备成功登录后，设备状态从 **未激活** 变成 **在线** 状态。此时设备可以上送数据，如果一段时间内不上送数据，设备状态变成 **离线**。

  如需采用动态激活方式，你需要在产品配置中开启 **动态激活**。

- **静态激活**：静态激活的过程如下：

  1. 设备发送携带`ProductKey`，`DeviceKey`，`DeviceSecret`的登录请求。

  2. 设备登录成功以后，设备状态从 **未激活** 变成 **在线** 状态。此时设备可以上送数据，如果一段时间内不上送数据，设备状态变成 **离线**。

  如果发现设备异常，或者你不希望接收该设备的数据，可以将其置为 **禁用**。此时设备将自动下线，并处于 **离线** 状态。

设备整体的状态可以分为操作状态、激活状态、连接状态三个维度，如下表所示：

.. list-table::

   * - 操作状态
     - 激活状态
     - 连接状态
   * - 禁用
     - --
     - 离线
   * - 启用
     - 未激活
     - --
   * - --
     - 已激活
     - 在线
   * - --
     - --
     - 离线

## 设备认证过程<authentication>

设备连接主要认证过程如下：

1. 云端预注册设备。

2. 在edge端配置云端注册的设备信息，主要为烧录设备三元组。

3. Edge上电、联网、尝试登录。云端验证设备三元组：

   - 成功：返回成功，告知设备发送数据。
   - 失败：返回失败，中断连接。

4. 设备以MQTT协议发送数据。

5. 云端以MQTT协议下发命令。

6. 设备端响应云端请求。

具体流程图如下：

.. image:: ../media/secret_communication.png

## 基于证书的双向认证<Certificatebase>

基于密钥的认证是使用设备三元组进行设备身份认证，是一种单向的认证机制，即IoT Hub校验设备是否可信。但设备并不校验对端接受数据的IoT Hub是否可信。如果开启双向认证，则需要使用 **基于证书的双向认证** 机制。

安全是IoT系统中至关重要的一点。在产品配置中启用证书身份验证后，EnOS会强制执行以下安全方案用以保护EnOS edge和EnOS IoT Hub之间的连接：

- EnOS edge和EnOS IoT Hub之间的通信强制使用基于证书的双向认证。
- 支持使用RSA算法验证签名，强制使用RSA 2048位的密钥类型。

讨论基于证书的双向认证机制之前，你需要了解以下概念：

- **证书颁发机构**：数字证书发行机构，能够颁发CA证书。
- **CA证书**：是一种数字证书，该证书的持有者可对其他证书进行签名。它符合[IETF RFC 5280](https://tools.ietf.org/html/rfc5280)标准定义的证书格式。
- **设备证书**：由CA证书签发的证书。设备证书的生成流程如下：
  - 基于设备信息产生一对密钥对。
  - 基于密钥对产生证书请求文件CSR。
  - 将设备CSR文件发送给CA。
  - CA证书签发并注册设备证书并在CA注册此设备证书。设备证书同时也会保存在设备端。

EnOS提供CA证书服务，更多内容参考[X.509证书服务](https://docs.eniot.io/docs/enos/zh_CN/latest/security/x509_ca/index.html#)。

如果开启基于证书的双向认证方式，我们推荐以下最佳实践：
  - 建议为每个设备提供一个唯一的证书，以便进行精细的管理，如证书撤销。
  - 设备必须支持更换证书，以确保证书过期时仍能顺畅运行。

### 设置证书<setup>

下图为基于X.509证书的edge和IoT Hub之间的安全通信过程。   

**1. IoT Hub获取X.509证书**

.. image:: ../media/certificate_service_secure_communication_01.png

1a. IoT Hub在本地创建密钥对和证书请求文件（CSR文件），使用X.509证书服务的API获取包含CSR的X.509证书。

1b. EnOS CA颁发X.509证书并将证书发送至IoT Hub。

1c. IoT Hub接收并存储X.509证书。

**2. Edge获取X.509证书**

.. image:: ../media/certificate_service_secure_communication_02.png

2a. Edge设备出厂预烧录了产品证书（`ProductKey`，`ProductSecret`）和edge设备序列号（SN）。设备上电联网以后，上报产品证书以及序列号至云端去动态激活。如果云端鉴权认证通过，会返回`DeviceSecret`至edge。

2b. 在IoT Hub上，使用edge的设备序列号作为`DeviceKey`预注册edge设备，可以在EnOS控制台注册设备，也可以通过调用REST API接口注册设备。

2c. Edge接收由IoT Hub返回的信息，创建密钥对和CSR文件，调用API获取它的X.509证书。同时使用设备三元组登录至云端，设备首次登录成功后会被激活。

2d. IoT Hub接收到从edge发来的CSR文件，在验证完身份后，将CSR文件转发至EnOS CA.

2e. EnOS CA接收到CSR文件，颁发edge证书并发送至IoT Hub。

2f. IoT Hub接收到已签发的X.509证书，把证书与设备ID绑定，然后将edge证书发送至edge。

2g. Edge接收到edge证书，需要保存到安全的本地库中。例如，保存至可信平台模块（TPM）。

#### 建立通信<communication>

下图为进行基于证书认证地过程：

**3. Edge与IoT Hub之间基于证书的双向认证的通信**

.. image:: ../media/certificate_service_secure_communication_03.png

3a. Edge认证IoT Hub端的证书。

3b. IoT hub认证edge端的证书

当3a和3b步骤中的TSL握手都成功后，Egde与IoT Hub之间的TLS连接即建立完成。

3c. 在TLS通道中，edge以MQTT协议传送设备遥测信息。

3d. 在TLS通道中，IoT Hub以MQTT协议传送配置和控制信号。

#### 撤销证书<revocation>

在某些情况下，用户需要撤销edge的X.509证书.

下图为撤销证书的过程。

**4. IoT Hub 撤销edge端的X.509证书**

.. image:: ../media/certificate_service_secure_communication_04.png

4a. IoT Hub调用撤销API来撤销edge端的带序列号的X.509证书。

4b. EnOS CA接收到来自IoT hub的请求后，验证身份，撤销证书，以及升级CRL.

### Edge安全的最佳实践<bestpractices>

在基于证书的安全连接中，参考以下最佳实践来确保edge的安全：

- 为edge创建私钥，并将私钥存放至一个安全地库。例如：TPM。
- 在与IoT Hub通信时使用TLS 1.2协议，并验证服务器证书的有效性。
- 每个edge都必须拥有唯一的公钥和私钥对。
- 由IoT Hub认证的密钥不能用于其他目的或通过其他协议进行通信。
- 当edge被重置时，密钥必须被撤回。
- 当你的edge在操作系统上运行时，确保你的操作系统上有安全机制。例如，防火墙。
- 确保你有方法来升级根CA根证书与CRL。
- 确保edge上的时钟不被篡改。

## 相关信息<relatedinformation>

- [创建产品](../howto/device/manage/creating_product)
- [创建设备](../howto/device/manage/creating_device).
- [实现基于证书的双向认证快速入门 (Java)](../quickstart/gettingstarted_java_ssl_connection)