# 4. 商业版Hadoop概述

目前大数据集群管理方式分为**手工方式（Apache hadoop）和工具方式（Ambari + hdp 和Cloudera Manger + CDH）**。**手工部署**需配置太多参数，但是，好理解其原理，建议初学这样做，能学到很多。该方式啊，均得由用户执行，细节太多，切当设计多个组件时，用户须自己解决组件间版本兼容问题。

**工具部署**比如Ambari或Cloudera Manger。（当前两大最主流的集群管理工具，前者是Hortonworks公司，后者是Cloudera公司）使用工具来，可以说是一键操作，难点都在工具Ambari或Cloudera Manger本身部署上。

**手工方式与工具方式的比对**

|  | 手工方式 | 工具方式 |
| :--- | :--- | :--- |
| 难易度 | 难度较大 | 简单、易行 |
| 兼容性 | 需求自己解决组件兼容性问题 | 自动安装兼容组件 |
| 组件支持数 | 支持全部组件 | 支持常用组件 |
| 优点 | 对组件和集群管理强 | 简单、容易、可行 |
| 缺点 | 组件繁多，较为复杂 | 屏蔽细节太多，妨碍组件理解 |

**不同工具方式的比对**

|  | Cloudera Manger | Ambari |
| :--- | :--- | :--- |
| 所属机构 | Cloudera | Hortonwork |
| 开源性 | 商用 | 开源 |
| 社区支持 | 不支持 | 支持 |
| 易用性、稳定性 | 易用、稳定 | 较易用，较稳定 |
| 市场占有率（国内） | 高 | 较高 |

常见的情况是，Cloudera Manger 去部署CDH, Ambari去部署HDP， 当然，两者也可以互相，也可以去部署Apache Hadoop

本节我们首先以Ambari为例，讲解如何使用工具管理管理Hadoop组件。

