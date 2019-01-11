# 资产树

资产树是EnOS平台设备管理服务的一个重要功能。资产树主要面向资产所有者，他们了解企业资产管理的业务，可以基于资产树快速创建管理资产的拓扑结构。

同时资产树功能与设备接入功能是解耦的，设备的预配与接入不依赖于资产树。你可以先接入设备，然后将接入设备绑定到资产树的节点。你也可以先创建资产树，然后再接入设备。通常情况下，接入设备都会绑定在资产树的某一节点当中。

## 概念

**资产树(AssetTree)**

资产树是资产与资产之间层级关系的一种组织方式。一个资产可以出现在多棵资产树当中，以满足对资产多种维度的管理。多棵树的主要使用场景如下：
- 按照地理位置划分：国家、省、市、区
- 按照领域划分：风电、光伏、储能
- 按照设备类型划分：风机、逆变器、汇流箱

如上述三种资产组织方式，可以构建三棵资产树，而资产树下的资产可以是相同的。

**节点(Node)**

直接挂载在资产树上的，是资产树节点，节点最多关联1个资产，也可以不关联资产。

资产树上的每一个节点都是唯一，但是不同的节点可以关联相同的资产，这样就实现了一个资产可以出现在不同资产树、或者同一个资产树的不同节点当中。资产树、节点、资产、设备关系如下图所示：

.. image:: ../media/asset_tree.png
   :alt: 图：资产树、资产、节点示意图
   :width: 780px

## 相关信息<relatedinformation>

- [快速入门资产树管理](gettingstarted_assettree)