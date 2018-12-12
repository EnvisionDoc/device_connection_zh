# 安全认证机制概述

为保证设备与传输的安全，设备接入EnOS IoT Hub之前，需要经过鉴权。EnOS支持**基于密钥的单向认证**和**基于证书的双向认证**两种认证模式。

## 基于密钥的单向认证<secretbase>

讨论基于密钥的单向认证机制之前，你需要了解以下概念：
- **产品密钥证书**:`ProductKey`和、`ProductSecret`。
  - `ProductKey`:是EnOS为产品颁发的全局唯一标识。
  - `ProductSecret`:由EnOS颁发的产品密钥，与`ProductKey`成对出现。常用于设备的动态激活，在设备初次连接EnOS Cloud时使用`ProductKey`、`ProductSecret`和`DeviceKey`可从云端获取`DeviceSecret`用于设备激活。
- **设备密钥证书**:`DeviceKey`和`DeviceSecret`。
  - `DeviceKey`:在注册设备时，自定义或系统自动生成的设备标识，具备OU内的唯一性。
  - `DeviceSecret`:由EnOS颁发的设备密钥，和`DeviceKey`成对出现。
- **设备三元组**:包含`ProductKey`、`DeviceKey`和`DeviceSecret`三要素，主要用于**基于密钥的认证**。

基于密钥的认证机制为系统默认强制执行。更多信息，参考[基于密钥的认证](secretbased_authentication)。


## 基于证书的双向认证<Certificatebase>

基于密钥的认证是使用设备三元组进行设备身份认证，是一种单向的认证机制，即IoT Hub校验设备是否可信。但设备并不校验对端接受数据的IoT Hub是否可信。如果开启双向认证，则需要使用**基于证书的双向认证**机制。

讨论基于证书的双向认证机制之前，你需要了解以下概念：
- **证书颁发机构**:数字证书发行机构，能够颁发CA证书。
- **CA证书**:是一种数字证书，该证书的持有者可对其他证书进行签名。它符合[IETF RFC 5280](https://tools.ietf.org/html/rfc5280)标准定义的证书格式。
- **设备证书**:设备证书是由CA证书签发的证书。设备证书的生成流程如下：
  - 基于设备信息产生一对密钥对。
  - 基于密钥对产生证书请求文件CSR。
  - 将设备CSR文件发送给CA。
  - CA证书签发设备证书并在CA注册此设备证书。设备证书同时也会保存在设备端。

EnOS提供CA证书服务，更多内容参考：[X.509 Certificate Service in EnOS](https://docs.envisioniot.com/docs/enos/zh_CN/latest/security/x509_ca/index.html#)。

如果开启基于证书的双向认证方式，我们推荐以下最佳实践：
  - 建议为每个设备提供一个唯一的证书，以便进行精细的管理，如证书撤销。
  - 设备必须支持更换证书，以确保证书过期时仍能顺畅运行。

基于证书的双向认证机制，更多内容参考:[基于证书的双重认证](certificatebased_authentication).
