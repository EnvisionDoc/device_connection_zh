# 设备端协议开发SDK参考

EnOS提供了已经封装了设备端与云端交互的协议的设备端SDK，可以直接使用设备端SDK进行规约开发。如果已提供的设备端SDK不能满足需求，可以参考本文，设备端协议规范了设备与云端之间的MQTT通讯数据传输格式，只需遵循该协议，就能完成设备接入。


## 协议参数说明

在介绍设备端协议前，先对通用的参数进行说明：

| 参数    | 取值    | 说明                                                         |
| ------- | ------- | ------------------------------------------------------------ |
| id      | String  | 消息ID号，保留值                                             |
| version | String  | 协议版本号，保留值                                           |
| params  | Json    | 请求参数，Json格式，其参数视请求的不同，可以为数组类型或者字典类型 |
| method  | String  | 请求或者指令的方法名                                         |
| code    | Integer | 结果返回码，结果返回码继承云端协议返回码。                   |
| data    | Json    | 返回结果，Json格式，其数据格式时返回结果的不同，可以为数组格式或者字典格式 |


## 当前支持的协议格式

设备端协议从消息的收发角度可以分为上行请求和下行指令。 目前设备端协议支持的协议格式如下：

- 上行请求
  * [子设备身份注册](#sub_device_reg)
  * [添加设备拓扑关系](#add_topo)
  * [删除拓扑关系](#del_topo)
  * [获取设备拓扑](#get_topo)
  * [子设备上线](#sub_login)
  * [子设备下线](#sub_logout)
  * [标签信息上报](#update_tag)
  * [删除标签信息](#del_tag)
  * [测点上报](#measurepoint_post)
  * [事件上报](#event_post)
  * [透传消息上报](#raw_post)
  * [获取TSL模型](#get_tsltemplate)

- 下行指令
  * [设备测点获取](#get_measurepoint)
  * [设备测点设置](#set_measurepoint)
  * [设备服务触发](#invoke_service)
  * [设备透传指令](#raw_down)
  * [设备禁用/启用/删除](#device_disable)



## 上行请求

### 子设备身份注册

上行请求

* request TOPIC:/sys/{productKey}/{deviceKey}/thing/sub/register
* REPLY TOPIC: /sys/{productKey}/{deivceKey}/thing/sub/register_reply

request请求
```json
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "productKey": "1234556554",
 "deviceAttributes": {
 "color":"red"
 },
 "deviceKey" : "deviceKey1234",
 "deviceName": "deviceName1234",
 "deviceDesc": "deviceDesc1234"
 }
 ],
 "method": "thing.sub.register"
}
```
response响应
```
{
 "id": "123",
 "code": 200,
 "data": [
 {
 "iotId": "12344",
 "productKey": "1234556554",
 "deviceKey": "deviceKey1234",
 "deviceSecret": "xxxxxx"
 }
 ]
}
```
#### 参数说明


| 参数         | 类型               | 说明                        |
| ------------ | ------------------ | --------------------------- |
| id           | Long               | 消息ID号，保留值            |
| version      | String             | 协议版本号，目前协议版本1.0 |
| params       | List               | 设备动态注册的参数          |
| deviceKey    | String             | 子设备key，request可不填    |
| deviceName   | String             | 子设备的名称，request可不填 |
| deviceDesc   | String             | 子设备描述，request可不填   |
| productKey   | String             | 子设备的产品Key             |
| iotId        | String             | 设备的唯一标识Id            |
| deviceSecret | String	设备秘钥    |                             |
| method       | String             | 请求方法                    |
| code         | Integer            | 结果信息                    |


### 添加设备拓扑关系
* 请求TOPIC: /sys/{productKey}/{deviceName}/thing/topo/add
* 返回TOPIC: /sys/{productKey}/{deviceName}/thing/topo/add_reply

request请求
```
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "deviceKey": "deviceName1234",
 "productKey": "1234556554",
 "sign": "xxxxxx",
 "signmethod": "hmacSha1",
 "timestamp": "1524448722000",
 "clientId": "xxxxxx"
 }
 ],
 "method": "thing.topo.add"
}
```
response响应
```
{
 "id": "123",
 "code": 200,
 "data": {}
}
```

#### 参数说明

| 参数       | 类型    | 说明                                                  |
| ---------- | ------- | ----------------------------------------------------- |
| id         | Long    | 消息ID号，保留值                                      |
| version    | String  | 协议版本号，目前协议版本1.0                           |
| params     | List    | 请求入参                                              |
| deviceKey  | String  | 设备的Key                                             |
| productKey | String  | 设备的产品Key                                         |
| sign       | String  | 签名                                                  |
| signmethod | String  | 签名方法，支持hmacSha1,hmacSha256,hmacMd5, Sha256     |
| timestamp  | String  | 时间戳                                                |
| clientId   | String  | 本地标记，非必填，可以和productKey&deviceName保持一致 |
| code       | Integer | 结果信息， 200表示成功                                |

>  注意
>  加签内容为将所有提交给服务器的参数（sign,signMethod除外）按照字母顺序排序, 然后将参数和值依次拼接（无拼接符号）。然后对加签内容，使用signMethod指定的加签算法进行加签。
>  如按照示例中rerequest请求，对request请求中的params中的参数进行加签.
>  sign= hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp1524448722000)

### 删除拓扑关系

- 请求TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/delete
- 返回TOPIC: /sys/{productKey}/{deviceKey}/thing/topo/delete_reply

Request请求
```
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "deviceKey": "deviceKey1234",
 "productKey": "1234556554"
 }
 ],
 "method": "thing.topo.delete"
}
```
response响应
```
{
 "id": "123",
 "code": 200,
 "data": {}
}
```

#### 参数说明

| 参数       | 类型    | 说明                        |
| ---------- | ------- | --------------------------- |
| id         | Long    | 消息ID号，保留值            |
| version    | String  | 协议版本号，目前协议版本1.0 |
| params     | List    | 请求参数                    |
| deviceKey  | String  | 设备Key                     |
| productKey | String  | 设备产品Key                 |
| method     | String  | 请求方法                    |
| code       | Integer | 结果信息， 200表示成功      |

###  获取设备拓扑关系

- 请求topic: /sys/{productKey}/{deviceKey}/thing/topo/get
- 返回topic: /sys/{productKey}/{deviceKey}/thing/topo/get_reply

request请求
```
{
 "id": "123",
 "version": "1.0",
 "params": {},
 "method": "thing.topo.get"
}
```
response响应
```
{
 "id": "123",
 "code": 200,
 "data": [
 {
 "deviceKey": "deviceKey1234",
 "productKey": "1234556554"
 }
 ]
}
```

#### 参数说明
| 参数       | 类型    | 说明                        |
| ---------- | ------- | --------------------------- |
| id         | Long    | 消息ID号，保留值            |
| version    | String  | 协议版本号，目前协议版本1.0 |
| params     | Object  | 请求参数，可为空            |
| method     | String  | 请求方法                    |
| deviceKey  | String  | 子设备的key                 |
| productKey | String  | 子设备的产品Key             |
| code       | Integer | 结果信息， 200表示成功      |

### 子设备上线

子设备上线需要确保身份已经注册、已添加拓扑关系。因为子设备上线，云端需要根据拓扑关系进行身份校验，以确定子设备是否具备复用网关通道的能力。

- 请求TOPIC: /ext/session/{productKey}/{deviceKey}/combine/login
- 返回TOPIC: /ext/session/{productKey}/{deviceKey}/combine/login_reply

request请求
```
{
 "id": "123",
 "params": {
 "productKey": "123",
 "deviceKey": "test",
 "clientId": "123",
 "timestamp": "123",
 "signMethod": "hmacmd5",
 "sign": "xxxxxx",
 "cleanSession": "true"
 }
}
```
response响应
```
{
 "id":"123",
 "code":200,
 "message":"success"
 "data": {
      "productKey": "123",
      "deviceKey": "test"
  }
}
```

> 签名方法
> 加签内容为将所有提交给服务器的参数（sign,signMethod除外）按照字母顺序排序, 然后将参数和值依次拼接（无拼接符号）。 然后对加签内容，使用signMethod指定的加签算法进行加签。
> sign= hmac_md5(deviceSecret, cleanSessiontrueclientId123deviceNametestproductKey123timestamp123)

#### 参数说明

| 参数         | 取值    | 说明                                                         |
| ------------ | ------- | ------------------------------------------------------------ |
| id           | Long    | 消息ID号，保留值                                             |
| params       | List    | 请求入参                                                     |
| deviceKey    | String  | 子设备的设备键值Key                                          |
| productKey   | String  | 子设备所属的产品Key                                          |
| sign         | String  | 子设备签名，规则与网关相同                                   |
| signmethod   | String  | 签名方法，支持hmacSha1,hmacSha256,hmacMd5, Sha256            |
| timestamp    | String  | 时间戳                                                       |
| clientId     | String  | 设备端标识，可以写productKey&deviceName                      |
| cleanSession | String  | 如果是true，那么清理所有子设备离线消息，即QoS1的所有未接收内容 |
| code         | Integer | 结果信息， 200表示成功                                       |

> 注意
> 网关下同时在线的子设备数目不能超过200，超过后，新的子设备上线请求将被拒绝。

### 子设备下线

- 请求TOPIC: /ext/session/{productKey}/{devuceKey}/combine/logout
- 返回TOPIC: /ext/session/{productKey}/{deviceKey}/combine/logout_reply

request请求
```
{
 "id": 123,
 "params": {
 "productKey": "xxxxx",
 "deviceName": "xxxxx"
 }
}
```
response响应
```
{
 "id": "123",
 "code": 200,
 "message": "success",
 "data": {
      "productKey": "xxxxx",
      "deviceKey": "xxxxx"
  }
}
```

#### 参数说明

| 参数       | 取值    | 说明                     |
| ---------- | ------- | ------------------------ |
| id         | Long    | 消息ID号，保留值         |
| params     | List    | 请求入参                 |
| deviceKey  | String  | 子设备的设备名称         |
| productKey | String  | 子设备所属的产品Key      |
| code       | Integer | 结果信息， 200表示成功   |
| message    | String  | 结果信息                 |
| data       | String  | 结果中附加信息，json格式 |

设备标签
### 标签信息上报

- 请求TOPIC: /sys/{productKey}/{deviceKey}/thing/deviceinfo/update
- 返回TOPIC： /sys/{productKey}/{deviceKey}/thing/deviceinfo/update_reply

request请求
```
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "tagKey": "Temperature",
 "tagValue": "36.8"
 }
 ],
 "method": "thing.deviceinfo.update"
}
```
response响应
```
{
 "id": "123",
 "code": 200,
 "data": {}
}
```

#### 请求参数
| 参数      | 取值    | 说明                                                         |
| --------- | ------- | ------------------------------------------------------------ |
| id        | Long    | 消息ID号，保留值                                             |
| version   | String  | 协议版本号，目前协议版本1.0                                  |
| params    | Object  | 请求参数, params元素个数不超过200个                          |
| method    | String  | 请求方法                                                     |
| attrKey   | String  | 标签的名称 <br/>· 长度不超过100字节 <br/>· 仅允许字符集为[a-z, A-Z, 0-9]和下划线 <br/>· 首字符必须是字母或者下划线 |
| attrValue | String  | 标签的值                                                     |
| code      | Integer | 结果信息， 200表示成功                                       |

### 删除标签信息

- 请求TOPIC /sys/{productKey}/{deviceName}/thing/deviceinfo/delete
- 返回TOPIC /sys/{productKey}/{deviceName}/thing/deviceinfo/delete_reply

request请求
```
{
 "id": "123",
 "version": "1.0",
 "params": [
 {
 "tagKey": "Temperature"
 }
 ],
 "method": "thing.deviceinfo.delete"
}
```
response响应
```
{
 "id": "123",
 "code": 200,
 "data": {}
}
```

#### 参数说明：
| 参数      | 取值    | 说明                                                         |
| --------- | ------- | ------------------------------------------------------------ |
| id        | Long    | 消息ID号，保留值                                             |
| version   | String  | 协议版本号，目前协议版本1.0                                  |
| params    | List    | 请求参数                                                     |
| method    | String  | 请求方法                                                     |
| attrKey   | String  | 标签的名称 <br/>· 长度不超过100字节  <br/>· 仅允许字符集为[a-z, A-Z, 0-9]和下划线  <br/>· 首字符必须是字母或者下划线 |
| attrValue | String  | 标签的值                                                     |
| code      | Integer | 结果信息， 200表示成功                                       |

###  测点上报

- 请求TOPIC: /sys/{productKey}/{deviceKey}/thing/event/property/post
- 返回TOPIC: /sys/{productKey}/{deviceKey}/thing/event/property/post_reply
  request请求
```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"measurepoints": {
			"Power": {
				"value": "1.0",
				"quality": "9"
			},
			"temp": 1.02,
			"branchCurr": [
				"1.02", "2.02", "7.93"
			]
		}
		"time": 123456
	}
},
"method": "thing.event.measurepoint.post"
}
```
response响应
```
{
"id": "123",
"code": 200,
"data": {}
}
```

### 事件上报（非透传）

- 请求TOPIC: /sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post
- 返回REPLY TOPIC: /sys/{productKey}/{deviceKey}/thing/event/{tsl.event.identifier}/post_reply
  request请求
```
{
  "id": "123",
  "version": "1.0",
 "params": {
		"events": {
			"Power": {
				"value": "1.0",
				"quality": "9"
			},
			"temp": 1.02,
			"branchCurr": [
				"1.02", "2.02", "7.93"
			]
		},
		"time": 123456
	},
  "method": "thing.event.{tsl.event.identifier}.post"
}
```
response响应
```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

### 透传消息上报

当用户上报原始数据如二进制数据流，我们可以通过透传消息体进行上报，设备端会将对应的二进制流发送至云端，云端会根据用户提交的脚本将二进制流转换成响应的标准数据格式。透传消息没有固定的消息体，透传消息的topic格式如下：

- 请求TOPIC: /sys/{productKey}/{deviceName}/thing/model/up_raw
- 返回TOPIC：/sys/{productKey}/{deviceName}/thing/model/up_raw_reply

### 获取TSL模型

- 请求TOPIC: /sys/{productKey}/{deviceName}/thing/tsltemplate/get
- 返回TOPIC：/sys/{productKey}/{deviceName}/thing/tsltemplate/get_reply
  request请求
```
{
  "id": "123",
  "version": "1.0",
  "params": {

  },
  "method": "thing.tsltemplate.get"
}
```
response响应
```
{
	id: '1',
	code: 200,
	data： {
		"tslModelId": "modelId",
		"tslModelName": "testmodel",
		"tslModelDesc": "1111111111",
		"tslModelCatagory": "testmodel",
		"tslAttributeMap": {
			"test": {
				"minimum": -2.147483648E9,
				"maximum": 2.147483647E9,
				"exclusiveMinimum": false,
				"exclusiveMaximum": false,
				"defaultValue": 1.0,
				"unit": {
					"unitId": "rad/s",
					"multiplier": "ONE"
				},
				"hasQuality": false,
				"name": "test",
				"desc": "1",
				"required": false,
				"accessMode": true,
				"tags": {
					"tagMap": {}
				},
				"identifier": "test"
			}
		},
		"tslMeasurepointMap": {},
		"tslServiceMap": {
			"service2": {
				"outputData": [],
				"inputData": [],
				"name": "service2",
				"required": false,
				"callType": "SYNC",
				"desc": "test",
				"identifier": "service2"
			},
			"service1": {
				"outputData": [],
				"inputData": [{
					"minimum": -2.147483648E9,
					"maximum": 2.147483647E9,
					"exclusiveMinimum": false,
					"exclusiveMaximum": false,
					"unit": {
						"multiplier": "ONE"
					},
					"hasQuality": false,
					"name": "test",
					"desc": "",
					"required": false,
					"accessMode": false,
					"tags": {
						"tagMap": {}
					},
					"identifier": "test"
				}],
				"name": "service1",
				"required": false,
				"callType": "ASYNC",
				"desc": "",
				"identifier": "service1"
			}
		},
		"tslEventMap": {},
		"tag": {
			"tagMap": {}
		},
		"allowAdditionalAttribute": false,
		"inheritedAttributeIds": [],
		"inheritedMeasurepointIds": [],
		"inheritedServiceIds": [],
		"inheritedEventIds": [],
		"extraInfo": {
			"createBy": "yz",
			"createTime": 1534318625917,
			"updateBy": "testuser",
			"updateTime": 1534318748279,
			"ns": "testns"
		}
	}
}
```
#### 参数说明

| 参数               | 取值 | 说明                   |
| ------------------ | ---- | ---------------------- |
| id                 | Long | 消息ID号，保留值       |
| tslAttributeMap    | Map  | tsl模型属性            |
| tslMeasurepointMap | Map  | 模型测点               |
| tslServiceMap      | Map  | 模型服务               |
| tslEventMap        | Map  | 模型事件               |
| tagMap             | Map  | 模型的tag              |
| extraInfo          | Map  | 模型创建更新的附加信息 |

## 下行指令

### 设备测点设置

设备收到测点设置指令后，可以对相应的测点该测点进行响应的处理。其消息协议如下：

- 请求TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/set
- 返回TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/set_reply

request请求
```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"temperature": "30.5"
	},
	"method": "thing.service.measurepoint.set"
}
```
response响应:
```
{
	"id": "123",
	"code": 200,
	"data": {}
}
```

### 设备测点获取

设备收到获取设备测点获取指令之后，通过reply消息向云端返回获取的测点信息。可以通过数据流转获取返回的测点信息。

- 请求TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/get
- 返回TOPIC: /sys/{productKey}/{deviceKey}/thing/service/measurepoint/get_reply

request请求
```
{
	"id": "123",
	"version": "1.0",
	"params": ["power", "temp"],
	"method": "thing.service.measurepoint.get"
}
```
response响应
```
{
	"id": "123",
	"code": 200,
	"data": {
		"power": "on",
		"temp": "23"
	}
}
```

### 设备服务触发

如果设备服务调用方式选择同步方式（物联网平台控制台中提前配置，产品服务定义中选择），云端直接使用[RRPC](#2.5)同步方式下行推送，

如果设备端服务调用方式选择异步方式（物联网平台控制台中提前配置，产品服务定义中选择），云端则采用异步方式下行推送，设备也采用异步方式返回。只有当前服务选择了异步方式，云端才会订阅该异步reply

异步服务设备协议如下：

- 请求Topic	/sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}
   返回Topic	/sys/{productKey}/{deviceKey}/thing/service/{tsl.service.identifier}_reply

request请求
```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"Power": "on",
		"WindState": "2"
	},
	"method": "thing.service.{tsl.service.identifier}"
}
```
response返回
```
{
	"id": "123",
	"code": 200,
	"data": {}
}
```

> 其中设备端可以根据执行结果的不同向云端返送不同的设备返回值，用于云端的集中处理。不从返回码代表的含义可以参照附录（#responseCode）返回码。


#### 设备透传指令

对于透传指令，云端可以自由地发送参数，甚至发送二进制格式的消息体。
- 请求TOPIC /sys/{productKey}/{deviceKey}/thing/service/down_raw
- 返回TOPIC /sys/{productKey}/{deviceKey}/thing/service/down_raw_reply

#### RRPC请求

RRPC请求是通过同步的方式将加你个云端指令下发给设备端，云端同步的响应返回的结果，同时云端会将同步消息的ID发给设备端，以保证云端消息的幂等，同时便于设备使用者进行消息的追溯。

- 请求TOPIC /sys/{productKey}/{deviceKey}/rrpc/requst/{messageId}
- 返回TOPIC /sys/{productKey}/{deviceKey}/rrpc/response/{messageId}

RRPC同步服务消息的请求格式的结构体与异步服务的结构体一致。

request请求
```
{
	"id": "123",
	"version": "1.0",
	"params": {
		"Power": "on",
		"WindState": "2"
	},
	"method": "thing.service.{tsl.service.identifier}"
}
```
response返回
```
{
	"id": "123",
	"code": 200,
	"data": {}
}
```

### 设备禁用、启用、删除

### 设备禁用
设备禁用设备禁用提供设备侧的通道能力，云端使用异步的方式推送该消息，设备订阅该Topic。具体禁用功能适用于网关类型设备，网关可以使用该功能禁用相应的子设备。

- 请求Topic	/sys/{productKey}/{deviceKey}/thing/disable
   返回Topic	/sys/{productKey}/{deviceKey}/thing/disable_reply

request请求
```
{
	"id": "123",
	"version": "1.0",
	"params": {},
	"method": "thing.disable"
}
```
response返回
```
{
	"id": "123",
	"code": 200,
	"data": {}
}
```
### 设备启用
设备启用设备启用提供设备侧的通道能力，云端使用异步的方式推送消息，设备订阅该topic。具体启用功能适用于网关类型设备，网关可以使用该功能恢复被禁用的子设备。

- 请求Topic	/sys/{productKey}/{deviceKey}/thing/enable
   返回Topic	/sys/{productKey}/{deviceKey}/thing/enable_reply
   request请求
```
{
	"id": "123",
	"version": "1.0",
	"params": {},
	"method": "thing.enable"
}
```
response返回
```
{
	"id": "123",
	"code": 200,
	"data": {}
}
```

### 设备删除

设备删除删除设备提供设备侧的通道能力，云端使用异步的方式推送消息，设备订阅该topic。具体删除设备的功能适用于网关类型设备，网关可以使用该功能删除相应的子设备。

- 请求Topic	/sys/{productKey}/{deviceKey}/thing/delete
   返回Topic	/sys/{productKey}/{deviceKey}/thing/delete_reply

request请求消息格式
```
{
	"id": "123",
	"version": "1.0",
	"params": {},
	"method": "thing.delete"
}
```
response 返回消息格式
```
{
	"id": "123",
	"code": 200,
	"data": {}
}
```
