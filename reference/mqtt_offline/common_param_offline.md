# 通用参数说明

以下表格列出了一些通用参数的说明。

.. list-table::
   :widths: auto

   * - 参数
     - 取值
     - 是否必需
     - 说明
   * - id
     - String
     - 可选
     - 消息ID号，保留值
   * - version
     - String
     - 必需
     - 协议版本号。
   * - params
     - JSON
     - 请求中必需
     - 请求中的参数。可以为int或dict格式
   * - method
     - String
     - 请求中必需
     - 请求方法。
   * - code
     - Integer
     - 响应中必需
     - 结果返回码，继承云端协议返回码
   * - data
     - JSON
     - 可选
     - 返回的详细信息。根据返回值的不同其可以为数组格式或者字典格式

<!--This is the End-->
