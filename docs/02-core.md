## 核心设计原理

> 我们的理念：行业需要什么，我们就做什么，用户需要什么，我们提供什么。

### 大数据痛点

- `Apache Hadoop`基于`hdfs`存储，基于`MapReduce`任务查询，并发数量较少，任务型驱动<b style="color:red">不够实时</b>
- `hdfs`文件存储基本存储单位为64M(`Hadoop2`中是128M)，如果是大量的小文件，会<b style="color:red">消耗大量内存</b>
- 基于 `Apache Hive`的查询引擎，涉及到`HSQL`语句转变成`MapReduce`任务来执行，<b style="color:red">严重影响并发与查询实时性</b>
- 基于`Apache Impala`的查询引擎，不依赖`MapReduce`任务，采用内存计算，<b style="color:red">对内存要求极高</b>
- 综上所述：<b style="color:red">大数据适合大文件持续任务化分析，不适合物联网碎片化数据存储</b>



### AIoTDB分布式集群设计原理

- 参照`Apache Hadoop`、`Apache Hive`、`Elastic Search`、`Apache IotDB`、`TD Engine`、`MySql`、`ETCD`、`Apache ApiSix`、`Linux`等设计原理，糅合功能优缺点，<b style="color:red">创造</b>`AIoTDB`
- 实时查询，采用拉取式查询，<b style="color:red">毫秒级查询</b>
- 分布式文件存储，集群去中心化自动文件负载
- `DataNode`文件块最小8kb占用空间，理论存储文件数<b style="color:red">4294967295</b>个
- `ManageNode`提供分布式集群节点`DataNode`的`Meta`信息，去除类`MapReduce`任务引擎，可选集成`Redis`加速`ASQL`选中执行器
- `ResourceNode`提供<b style="color:red">分布式数据代理节点</b>，结合`ManageNode`统一提供对外服务
- `独立网关`绑定域名，可对不同厂商提供<b style="color:red">云数据服务</b>
- 包含 <b style="color:red">事件驱动、离线计算、机器学习</b> 等可选服务



> 时间线划分原理

- 由`租户`、`项目`、`区域`、`设备`、`测点` 维度进行建立时间线，自动进行数据存储，无需用户干预。如：`t01.p01.a01.d01.p01`

  ```mermaid
  graph LR;
      租户-->项目;
      项目-->区域;
      区域-->设备;
      设备-->测点;
  ```

- 融合节点使用`root`标识，如（无区域）节点：`t01.p01.root.d01.p01`