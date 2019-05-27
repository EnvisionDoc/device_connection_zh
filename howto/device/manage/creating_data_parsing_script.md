# 解析非EnOS标准JSON格式的数据

对于使用透传/自定义格式传输数据的设备，你可以在EnOS Cloud编写脚本，解析设备数据。解析脚本支持使用JavaScript进行开发。

## 任务描述

由于低配置且资源受限或者对网络流量有要求的设备，不适合直接构造JSON数据和云端通信。因此，选择将数据透传到云端，由云端运行转换脚本将透传的数据转换成EnOS定义的JSON格式数据。云端向设备发送控制指令时，也可通过脚本将EnOS定义的JSON格式数据转换为设备能够理解的二进制数据进行下发。

EnOS Cloud为开发者提供了以下功能：
- 脚本在线编辑器，支持 JavaScript 语言，对语法进行静态检查；
- 支持保存草稿，并从草稿中恢复编辑，支持删除草稿；
- 支持对脚本的模拟数据调试，可输入上下行数据进行模拟转换，查看脚本运行结果；
- 提交脚本到运行环境，在设备上下行时进行调用；

数据解析脚本的调用流程：

1. 编辑解析脚本
2. 模拟运行
3. 提交脚本


## 开始前准备

选择 **设备管理 > 产品管理 > 创建产品**，在弹窗中填写相关信息，数据格式选择**自定义**，点击 **确定** 完成该产品的创建；

  .. image:: ../../../media/product_management_new_product.png

## 步骤1：编辑解析脚本

1. 产品创建完成后，点击 **查看**，进入产品详情页面，点击 **数据解析** 标签页；

  .. image:: ../../../media/product_detail_data_parsing.png

2. 在编辑器中使用JavaScript语言编写数据解析脚本：

数据解析脚本支持两个方法调用，对于设备上行数据，系统调用`rawDataToJsonStr`方法对透传数据进行解码；对于云端下行数据，系统调用`jsonStrToRawData`方法，将JSON 格式编码为设备可以理解的二进制编码；

.. note:: 使用解析脚本时，设备上下行消息请使用透传Topic。

上行

- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/model/up_raw`
- 响应TOPIC：`/sys/{productKey}/{deviceKey}/thing/model/up_raw_reply`

下行

- 请求TOPIC: `/sys/{productKey}/{deviceKey}/thing/model/down_raw`
- 响应TOPIC: `/sys/{productKey}/{deviceKey}/thing/model/down_raw_reply`

脚本解析中定义的两个方法如下：

```js
    /**
      *  将JSON协议的数据转换为设备能识别的格式数据，物联网平台给设备下发数据时调用
      *  入参：jsonStr json字符串    不能为空
      *  出参：rawData byte[]数组    不能为空
    **/
    function jsonStrToRawData(jsonStr) {
        return rawData;
    }

    /**
      * 将设备的自定义格式数据转换为JSON协议的数据，设备上报数据到物联网平台时调用
      * 入参：rawData byte[]数组    不能为空
      * 出参：jsonStr json字符串    不能为空
    **/
    function rawDataToJsonStr(rawData) {
        return jsonStr;
    }
```

点击 **保存草稿**，系统将保存本次编辑的结果；再次回到编辑界面时，将提示最近一次保存的草稿，您可以选择恢复编辑或删除草稿。

.. note:: - 删除的草稿无法恢复。
   - 草稿不会进入到脚本解析的运行环境中，保存草稿不会影响已经提交的正式脚本。
   - 每次保存草稿将覆盖上一次保存的草稿。


## 步骤2：模拟运行

在 **模拟输入** 中，输入模拟数据，并可选定模拟类型为 **设备上发数据** 或 **设备接收数据**。

点击 **运行**，系统将调用脚本模拟数据解析，并将运行结果显示在右侧结果区域中，如果存在运行错误，将提示您重新修改脚本代码后重新运行。

### 模拟设备上发数据解析

在对话框中填入透传二进制数据，单击 **运行** 后，系统会调用`rawDataToJsonStr`方法，将二进制透传数据按照脚本规则转换为JSON格式数据，您可以在 **运行结果** 区域看到透传数据的解析结果；

### 模拟设备接收数据编码

在对话框中填入JSON格式数据，单击 **运行** 后，系统会调用`JsonStrToRawData`方法，将JSON格式数据按照脚本规则编码为二进制透传数据，您可以在 **运行结果** 区域看到编码结果；


## 步骤3：提交脚本到运行环境

为避免错误提交脚本造成数据无法上下行，脚本运行通过后，才能提交到运行环境。脚本提交后不会立即运行，一般需要等待5分钟，正式环境才能完成脚本的更新。

.. note:: 提交脚本之前需要至少运行通过一次，才能提交。

## 脚本示例

### 上报测点信息

1. 为设备定义如下模型：

   .. csv-table::
      :widths: auto

      "测点标识符", "类型", "字节数"
      "mp_int", "int", "4"
      "mp_string", "string", "可变长度"
      "mp_double", "double", "8"
      "mp_array", "array", "可变长度"
      "mp_int_quality", "带有quality属性的integer", "5"

2. 为测点在二进制协议中定义编号如下：

   .. csv-table::
      :widths: auto

      "编号", "测点标识符"
      "0x01", "mp_int"
      "0x02", "mp_string"
      "0x03", "double"
      "0x04", "array"
      "0x05", "mp_int_quality"

   测点上报的二进制报文格式为：

   .. csv-table::
      :widths: auto

      "说明", "字节数"
      "消息类型，0x01表示测点上送 ", "1"
      "请求ID", "4"
      "测点1的标识符对应的编号", "1"
      "测点1长度", "2"
      "测点1的值", "根据测点类型的字节数定义"
      "测点2的标识符对应的编号", "1"
      "测点2长度", "2"
      "测点2的值", "根据测点类型的字节数定义"

   以此类推。   

3. 填写模拟数据：

   ```
     0x0100000014010004000025f502000a6162636465666768696a0300083ff23d70a3d70a3d0400140000000100000003000000050000000700000009050005000001a703
   ```
   示例脚本被执行后，输出的 JSON 内容为：

   ```json
        {
             "id": "20",
             "method": "thing.measurepoint.post",
             "version": "1.0",
             "params": {
                 "measurepoints": {
                     "mp_int": 9717,
                     "mp_string": "abcdefghij",
                     "mp_double": 1.14,
                     "mp_array": [ 1, 3, 5, 7, 9 ],
                     "mp_int_quality": {
                         "value": 423,
                         "quality": 3
                     }
                 },
                 "time": <EnOS接收到消息的系统时间>
             }
         }
   ```

### 设备接收数据

1. 设置测点。设置测点的二进制消息格式定义如下：

   .. csv-table::
      :widths: auto

      "说明", "字节数"
      "消息类型，0x02表示测点设置 ", "1"
      "请求ID", "4"
      "测点1的标识符对应的编号", "1"
      "测点1长度", "2"
      "测点1的值", "根据测点类型的字节数定义"
      "测点2的标识符对应的编号", "1"
      "测点2长度", "2"
      "测点2的值", "根据测点类型的字节数定义"

   如果有更多的测点需要设置，依此类推

2. 构造JSON格式的输入如下：

   ```json
         {
             "id": "20",
             "method": "thing.service.measurepoint.set",
             "version": "1.0",
             "params": {
                  "mp_int": 9717,
                  "mp_string": "abcdefghij",
                  "mp_double": 1.14,
                  "mp_array": [1, 3, 5, 7, 9],
                  "mp_int_quality": {
                      "value": 423,
                      "quality": 3
                  }
              }
          }
   ```

   示例脚本解析后，输出的二进制消息为：

   ```
   0x020000001401000025f502000a6162636465666768696a033ff23d70a3d70a3d040014000000010000000300000005000000070000000905000001a703
   ```

### 调用设备服务

1. 定义服务模型如下：

   .. csv-table::
      :widths: auto

      "服务标识符", "类型", "字节数", "输出参数", "输出参数模型"
      "mp_int", "input_int", "int", "output_int", "int"

2. 定义调用该服务的消息格式定义如下：

   .. csv-table::
      :widths: auto

      "说明", "字节符"
      "要调用的服务类型，0x11表示调用service1这个设备服务  ", "1"
      "请求ID", "4"
      "输入参数input_int的值", "4"

3. 构造JSON格式的服务请求作为输入：

   ```json
        {
        "method":"thing.service.service1",
        "id":1234,
        "version":"1.0",
        "params": {
            "input_int":12
            }
        }
   ```

   示例脚本解析后，输出的二进制消息为：

   ```
   0x11000004d2c80000000e
   ```

### 解析脚本完整示例代码

```js
    "use strict";

    /**
     * 常量定义
     */

    // 测点上报方法的编码
    var CODE_MEASUREPOINT_POST = 0x01;
    //标准JSON格式method，设备上传测点数据到云端
    var METHOD_MEASURE_POINT_POST = 'thing.measurepoint.post';

    //标准JSON格式method， 云端下发设置测点指令到设备端
    var CODE_MEASUREPOINT_SET = 0x02;
    var METHOD_MEASUREPOINT_SET = 'thing.service.measurepoint.set';
    var CODE_MEASUREPOINT_SET_REPLY = CODE_MEASUREPOINT_SET;

    // 标准JSON格式协议的method，云端下发服务调用到设备端；service1是服务的identifier
    var CODE_SERVICE_1 = 0x11; // service1服务的编码
    var METHOD_SERVICE_1 = 'thing.service.service1';
    var CODE_SERVICE_1_REPLY = CODE_SERVICE_1;


    /**
     * rawDataToJsonStr function 定义
     * 上行消息时调用此方法，将透传数据转换成Json格式数据
     * 入参：rawData    byte[]数组    不能为空
     * 出参：jsonStr    json字符串    不能为空
     */
    function rawDataToJsonStr(bytes) {
        var uint8Array = new Uint8Array(bytes.length);
        for (var i = 0; i < bytes.length; i++) {
            uint8Array[i] = bytes[i] & 0xff;
        }
        var dataView = new DataView(uint8Array.buffer, 0);
        var method_code = uint8Array[0]; // method code
        var jsonMap = {}; // 返回的json对象
        if (method_code === CODE_MEASUREPOINT_POST) {
            jsonMap = measurepoint_post(dataView);
        } else if (method_code === CODE_MEASUREPOINT_SET_REPLY) {
            jsonMap = measurepoint_set_reply(dataView);
        } else if (method_code === CODE_SERVICE_1_REPLY) {
            jsonMap = service_1_invoke_reply(dataView);
        }
        // return jsonStr;
        return JSON.stringify(jsonMap);
    }

    // measurepoint 编号
    var codeMapMeasurepoint = {
        0x01: "mp_int",
        0x02: "mp_string",
        0x03: "mp_double",
        0x04: "mp_array",
        0x05: "mp_int_quality"
    };

    /**
     * 测点上报模拟数据
     * 输入：
       0x0100000014010004000025f502000a6162636465666768696a0300083ff23d70a3d70a3d0400140000000100000003000000050000000700000009050005000001a703
     * 输出：
       {
           "id": "20",
           "method": "thing.measurepoint.post",
           "version": "1.0",
           "params": {
               "measurepoints": {
                   "mp_int": 9717,
                   "mp_string": "abcdefghij",
                   "mp_double": 1.14,
                   "mp_array": [ 1, 3, 5, 7, 9 ],
                   "mp_int_quality": {
                       "value": 423,
                       "quality": 3
                   }
               },
               "time": 1550894467486
           }
       }
     */
    function measurepoint_post(dataView) {
        var jsonMap = {};
        jsonMap['id'] = '' + dataView.getUint32(1); //JSON格式 - 标示该次请求id值
        jsonMap['method'] = METHOD_MEASURE_POINT_POST; //JSON格式 - 测点上报的method
        jsonMap['version'] = '1.0'; //JSON格式 - 协议版本号固定字段
        var params = {
            'measurepoints': {},
            'time': new Date().getTime()
        };
        var index = 5;
        for (; index < dataView.byteLength;) {
            var mp_code = dataView.getUint8(index); // 读取measurepoint code, 1个字节
            index += 1;
            var length = dataView.getInt16(index); // 读取value length, 2个字节
            index += 2;
            var value = null;
            if (mp_code === 0x01 && length === 4) {
                value = dataView.getInt32(index);
            } else if (mp_code === 0x02) {
                value = decodeString(dataView, index, length);
            } else if (mp_code === 0x03 && length === 8) {
                value = dataView.getFloat64(index);
            } else if (mp_code === 0x04) {
                value = decodeArray(dataView, index, length);
            } else if (mp_code === 0x05 && length === 5) {
                value = {};
                value.value = dataView.getInt32(index);
                value.quality = dataView.getInt8(index+4);
            }
            params.measurepoints[codeMapMeasurepoint[mp_code]] = value;
            index += length;
        }
        jsonMap['params'] = params; // JSON格式 - params标准字段
        return jsonMap;
    }

    /**
     * 测点设置的返回模拟数据
     * 输入：
       0x020000001400c8
     * 输出：
       {
           "id": "20",
           "code": "0",
           "data": ""
       }
     */
    function measurepoint_set_reply(dataView) {
        var jsonMap = {};
        jsonMap['id'] = '' + dataView.getUint32(1); //JSON格式 - 标示该次请求id值
        jsonMap['code'] = '' + dataView.getUint8(5);
        jsonMap['data'] = '';
        return jsonMap;
    }

    /**
     * 服务调用的返回模拟数据
     * 输入：
       0x11000004d2c80000000e
     * 输出：
       {
           "id": "1234",
           "code": "200",
           "data": {
               "output_int": 14
           },
           "message": ""
       }
     */
    function service_1_invoke_reply(dataView) {
        var jsonMap = {};
        jsonMap['id'] = '' + dataView.getUint32(1);
        jsonMap['code'] = '' + dataView.getUint8(5);
        var data = {};
        data['output_int'] = dataView.getInt32(6);
        jsonMap['data'] = data;
        var message = '';
        if (dataView.byteLength > 10) {
            message = decodeString(dataView, 10);
        }
        jsonMap['message'] = message;
        return jsonMap;
    }


    function decodeString(dataView, index, length) {
        var strArr = []; // new Uint8Array(length);
        for (var i = 0; i < length; i++) {
            strArr[i] = dataView.getInt8(i + index);
        }
        var str = String.fromCharCode.apply(String, strArr);
        return str;
    }

    function decodeArray(dataView, index, length) {
        var result = [];
        for (var i = 0; i < length; i += 4) {
            result = result.concat(dataView.getUint32(index + i));
        }
        return result;
    }


    /**
     * jsonStrToRawData function 定义
     * 下行消息时调用此方法，将Json格式数据转换成透传数据
     * 入参：jsonStr    json字符串    不能为空
     * 出参：rawData    byte[]数组    不能为空
     */
    function jsonStrToRawData(jsonStr) {
        var json = JSON.parse(jsonStr); // jsonStr to jsonObject
        var method = json['method']; // 获取method
        var rawData = [];
        if (isEmpty(method)) {
            rawData = reply(json)
        } else if (method === METHOD_MEASUREPOINT_SET) {
            rawData = measurepoint_set(json);
        } else if (method === METHOD_SERVICE_1) {
            rawData = service_1_invoke(json);
        }
        return rawData;
    }

    /**
     * 测点上报回复模拟数据
     * 输入：
       {
           "id":20,
           "code":200,
           "data":""
       }
     * 输出：
       0x0000001400c8
     */
    function reply(json) {
        var id = json['id']; // ['id'];
        var code = json['code'];
        var data = json['data'];
        var rawData = [];
        rawData = rawData.concat(buffer_int32(id));
        rawData = rawData.concat(buffer_int16(code));
        if (data !== '') {
            rawData = rawData.concat(data);
        }
        return rawData;
    }

    // 测点编号
    var measurepointMapCode = {
        "mp_int": 0x01,
        "mp_string": 0x02,
        "mp_double": 0x03,
        "mp_array": 0x04,
        'mp_int_quality': 0x05
    };

    /**
     * 测点设置模拟数据
     * 输入：
       {
            "id": "20",
            "method": "thing.service.measurepoint.set",
            "version": "1.0",
            "params": {
                "mp_int": 9717,
                "mp_string": "abcdefghij",
                "mp_double": 1.14,
                "mp_array": [1, 3, 5, 7, 9],
                "mp_int_quality": {
                    "value": 423,
                    "quality": 3
                }
            }
        }
     * 输出：
       0x020000001401000025f502000a6162636465666768696a033ff23d70a3d70a3d040014000000010000000300000005000000070000000905000001a703
     */
    function measurepoint_set(json) {
        var id = json['id'];
        var version = json['version']; // version字段，不在编码进rawData
        var rawData = [];
        rawData = rawData.concat(buffer_uint8(CODE_MEASUREPOINT_SET)); // command字段
        rawData = rawData.concat(buffer_int32(parseInt(id))); // JSON格式 'id'
        //按照自定义协议格式拼接 rawData
        var params = json['params'];
        for (var key in params) {
            rawData = rawData.concat(measurepointMapCode[key])
            if (key === 'mp_int') {
                rawData = rawData.concat(buffer_int32(params[key]))
            } else if (key === 'mp_string') {
                rawData = rawData.concat(buffer_string(params[key]));
            } else if (key === 'mp_double') {
                rawData = rawData.concat(buffer_double64(params[key]))
            } else if (key === 'mp_array') {
                var arr_value = params[key];
                rawData = rawData.concat(buffer_int16(arr_value.length * 4))
                for (var i = 0; i < arr_value.length; i++) {
                    rawData = rawData.concat(buffer_int32(arr_value[i]))
                }
            } else if (key === 'mp_int_quality') {
                var json_quality = params[key];
                rawData = rawData.concat(buffer_int32(json_quality['value']));
                rawData = rawData.concat(buffer_uint8(json_quality['quality']));
            }
        }
        return rawData;
    }

    /**
     * 服务调用模拟数据
     * 输入：
        {
            "method":"thing.service.service1",
            "id":1234,
            "version":"1.0",
            "params": {
                "input_int":12
            }
        }
     * 输出：
       0x11000004d20000000c
     */
    function service_1_invoke(json) {
        var id = json['id'];
        var version = json['version'];
        var payloadArray = [];
        payloadArray = payloadArray.concat(buffer_uint8(CODE_SERVICE_1)); // command字段
        payloadArray = payloadArray.concat(buffer_int32(parseInt(id))); // JSON格式 'id'
        var params = json['params'];
        var inparam_1 = params['input_int'];
        payloadArray = payloadArray.concat(buffer_int32(parseInt(inparam_1)))
        return payloadArray;
    }


    function isEmpty(obj) {
        if (obj === undefined || obj === null || obj === "") {
            return true;
        } else {
            return false;
        }
    }

    //以下是部分辅助函数
    function buffer_uint8(value) {
        var uint8Array = new Uint8Array(1);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setUint8(0, value);
        return [].slice.call(uint8Array);
    }

    function buffer_int16(value) {
        var uint8Array = new Uint8Array(2);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setInt16(0, value);
        return [].slice.call(uint8Array);
    }

    function buffer_int32(value) {
        var uint8Array = new Uint8Array(4);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setInt32(0, value);
        return [].slice.call(uint8Array);
    }

    function buffer_double64(value) {
        var uint8Array = new Uint8Array(8);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setFloat64(0, value);
        return [].slice.call(uint8Array);
    }

    function buffer_string(value) {
        var length = value.length;
        var uint8Array = new Uint8Array(length + 2)
        uint8Array[0] = Math.floor(length / 256);
        uint8Array[1] = length % 256;
        for (var i = 0; i < length; i++) {
            uint8Array[i + 2] = value.charCodeAt(i);
        }
        return [].slice.call(uint8Array);
    }

    /**
     *  以下是本地测试方法
     */
    function TestJsonToRaw() {
        var json = "{\n" +
            "    \"method\": \"thing.service.measurepoint.set\",\n" +
            "    \"id\": \"20\",\n" +
            "    \"params\": {\n" +
            "            \"mp_int\": 1163,\n" +
            "            \"mp_string\": \"abcdefg\"\n" +
            "    },\n" +
            "    \"version\": \"1.0\"\n" +
            "}";
        console.log(json);
        var rawData = jsonStrToRawData(json);
        console.log(rawData);
    }

    function TestRawToJson() {
        // 0x0100000014010004000025f502000a6162636465666768696a0300083ff23d70a3d70a3d0400140000000100000003000000050000000700000009050005000001a701
        var rawData = [
            0x01, // method, thing.measurepoint.post
            0x00, 0x00, 0x00, 0x14, // id
            0x01, // mp_int code
            0x00, 0x04, // length
            0x00, 0x00, 0x25, 0xf5, // value
            0x02, // mp_string code
            0x00, 0x0a, // length
            0x61, 0x62, 0x63, 0x64, 0x65, 0x66, 0x67, 0x68, 0x69, 0x6a, // value
            0x03, // mp_double code
            0x00, 0x08, // length
            0x3f, 0xf2, 0x3d, 0x70, 0xa3, 0xd7, 0x0a, 0x3d, // value
            0x04, // mp_array code
            0x00, 0x14, // length
            0x00, 0x00, 0x00, 0x01, 0x00, 0x00, 0x00, 0x03, 0x00, 0x00, 0x00, 0x05, 0x00, 0x00, 0x00, 0x07, 0x00, 0x00, 0x00, 0x09, // value
            0x05, // mp_int_quality
            0x00, 0x05, // length
            0x00, 0x00, 0x01, 0xa7, // value
            0x01 // quality
        ];
        var jsonMap = rawDataToJsonStr(rawData);
        console.log(jsonMap)
    }

    function TestService_1ToRawData() {
        var json = "{\n" +
            "    \"method\": \"thing.service.service1\",\n" +
            "    \"id\": \"20\",\n" +
            "    \"params\": {\n" +
            "            \"input_int\": 1163\n" +
            "    },\n" +
            "    \"version\": \"1.0\"\n" +
            "}";
        console.log(json);
        var rawData = jsonStrToRawData(json);
        console.log(rawData);
    }

    // 本地启动测试方法
    // TestRawToJson();
    // TestJsonToRaw();
    // TestService_1ToRawData();
```


## 脚本错误码说明

在线调试脚本时，可能会出现如下错误码：

.. list-table::
   :widths: auto

   * - 错误码
     - 错误消息
     - 说明
   * - 900
     - script service unavailable, internal error
     - 脚本服务内部错误
   * - 901
     - some params null
     - 调用RPC接口参数为空错误
   * - 902
     - param error
     - 参数错误
   * - 903
     - script exe failed
     - 脚本执行失败
   * - 904
     - script exe time
     - 脚本执行超时
   * - 905
     - script exe interrupted exception
     - 脚本执行被中断
   * - 906
     - script exception
     - 脚本异常
   * - 951
     - internal db operation error
     - 内部数据库操作错误

<!--end-->
