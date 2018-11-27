设备拓扑管理
============

子设备身份在EnOS 注册后，网关会向云端上报网关与子设备的拓扑关系，然后子设备可以上线。
上线过程中，EnOS Cloud会校验子设备的身份和与拓扑关系。验证合法，才会建立并绑定子设备逻辑通道至Edge物理通道上。


.. toctree::
   :maxdepth: 1

   add_topological
   delete_topological
   get_topological
