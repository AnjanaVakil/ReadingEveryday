分片（Partition）：解决数据集尺度与单机容量、负载不匹配的问题，分片之后可以利用多机容量和负载。
复制（Replication）：系统机器一多，单机故障概率便增大，为了防止数据丢失以及服务高可用，需要做多副本。

通常来说，数据系统在分布式系统中会有三级划分：数据集（如 Database、Bucket）——分片（Partition）——数据条目（Row、KV）。通常，每个分片只属于一个数据集，每个数据条目只属于一个分片。单个分片，就像一个小点的数据库。但是，跨分区的操作的，就要复杂的多。