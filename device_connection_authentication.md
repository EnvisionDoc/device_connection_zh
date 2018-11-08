# 设备认证机制

EnOS为设备颁发安全证书以保证设备连接的安全性。设备接入IoT Hub之前，需依据不同的认证机制上报其证书，云端认证证书通过后，设备方可接入EnOS。EnOS支持*设备唯一证书*和*产品唯一证书*两种设备认证机制。

讨论认证机制之前，你需要了解以下概念：

- `ProductKey`：是EnOS为产品颁发的全局唯一标识。
- `ProductSecret`： 由平台颁发的产品密钥，通常与ProductKey成对出现，可用于一型一密的认证方案。
- 产品证书： `ProductKey`和`ProductSecret`。
- `DeviceKey`：在注册设备时，自定义的或自动生成的设备名称，具备产品维度内的唯一性。
- `DeviceSecret`：EnOS为设备颁发的设备密钥，和DeviceName成对出现。
- 设备证书： `DeviceName`和`DeviceSecret`。

## 设备唯一证书 （Certificate-per-device）

每台设备烧录其产品型号对应的`ProductKey`及唯一识别自己的设备证书`DeviceKey`和`DeviceSecret`。

一般是线下通过后台开发者接口，或者控制台获取，然后烧录灌入到子设备中。设备与云端建连时携带`ProductKey`、`DeviceKey`和`DeviceSecret`进行接入认证，认证通过后云端完成设备激活才可传输数据。通过一机一密进行预配置及接入EnOS Cloud的过程称为*静态添加*。

安全性：相对较高

EnOS默认采用一机一密。

## 产品唯一证书 （Certificate-per-product）

同一产品型号的设备烧录相同产品证书`ProductKey`和`ProductSecret`。固件烧录后，设备激活时通过ProductKey从云端动态获取DeviceKey和DeviceSecret。通过一型一密进行预配置及接入EnOS Cloud的过程称为*动态添加*。

安全性：相对较弱
