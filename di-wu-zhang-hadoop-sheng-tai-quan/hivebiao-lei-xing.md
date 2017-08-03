## 5.4 Hive表类型

首先，Hive 没有专门的数据存储格式，也没有为数据建立索引，用户可以非常自由的组织 Hive 中的表，只需要在创建表的时候告诉 Hive 数据中的列分隔符和行分隔符，Hive 就可以解析数据。

其次，Hive 中所有的数据都存储在 HDFS 中，Hive 中包含以下数据模型：Table，External Table，Partition，Bucket。

**表（table）**: Hive 中的 Table 和数据库中的 Table 在概念上是类似的，每一个 Table 在 Hive 中都有一个相应的目录存储数据。这个目录可以通过${HIVE\_HOME}/conf/hive-site.xml配置文件中的 hive.metastore.warehouse.dir属性来配置，这个属性默认的值是/user/hive/warehouse\(这个目录在 HDFS上\)，我们可以根据实际的情况来修改这个配置。如果我有一个表wyp，那么在HDFS中会创建/user/hive/warehouse/wyp 目录\(这里假定hive.metastore.warehouse.dir配置为/user/hive/warehouse\);wyp表所有的数据都存放在这个目录中。这个例外是外部表。

**外部表\(External Table\)**:指向已经在 HDFS 中存在的数据，可以创建 Partition。它和 Table 在元数据的组织上是相同的，而实际数据的存储则有较大的差异。但是其数据不是放在自己表所属的目录中，而是存放到别处，这样的好处是如果你要删除这个外部表，该外部表所指向的数据是不会被删除的，它只会删除外部表对应的元数据;而如果你要删除表，该表对应的所有数据包括元数据都会被删除。

**分区\(Partition\)**:对应于数据库中的 Partition 列的密集索引，但是 Hive 中 Partition 的组织方式和数据库中的很不相同。在 Hive 中，表中的一个Partition 对应于表下的一个目录，所有的 Partition 的数据都存储在对应的目录中。Hive中的表的每一个分区对应表下的相应目录，所有分区的数据都是存储在对应的目录中。比如wyp 表有dt和city两个分区，则对应dt=20131218,city=BJ对应表的目录为/user/hive/warehouse /dt=20131218/city=BJ，所有属于这个分区的数据都存放在这个目录中

**桶\(Buckets\)**:对指定列计算 hash，根据 hash 值切分数据，目的是为了并行，每一个 Bucket 对应一个文件。对指定的列计算其hash，根据hash值切分数据，目的是为了并行，每一个桶对应一个文件\(注意和分区的区别\)。比如将wyp表id列分散至16个桶中，首先对id列的值计算hash，对应hash值为0和16的数据存储的HDFS目录为：/user /hive/warehouse/wyp/part-00000;而hash值为2的数据存储的HDFS 目录为：/user/hive/warehouse/wyp/part-00002。



![](/assets/5.4_1.png)

Table 的创建过程和数据加载过程（这两个过程可以在同一个语句中完成），在加载数据的过程中，实际数据会被移动到数据仓库目录中；之后对数据对访问将会直接在数据仓库目录中完成。删除表时，表中的数据和元数据将会被同时删除。

External Table 只有一个过程，加载数据和创建表同时完成（CREATE EXTERNAL TABLE ……LOCATION），实际数据是存储在 LOCATION 后面指定的 HDFS 路径中，并不会移动到数据仓库目录中。当删除一个 External Table 时，仅删除

