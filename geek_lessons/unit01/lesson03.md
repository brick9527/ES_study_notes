# 第三课 ES简介及其发展历史

# 一、新特性 5.X

- Lucene 6.X，性能提升，默认打分机制从TF-IDF改为BM 25
- 支持Ingest节点 / Painless Scripting / Completion suggested 支持 / 原生的Java REST客户端
- Type标记成deprecated，支持了Keyword的类型
- 性能优化
  - 内部引擎移除了避免同一文档并发更新的竞争锁，带来15% - 20%的性能提升
  - Instant aggregation，支持分片上聚合的缓存
  - 新增了Profile API

# 二、新特性 6.X

- Lucene 7.X
- 新功能
  - 跨集群复制（CCR）
  - 索引生命周期管理
  - SQL的支持
- 更友好的升级及数据迁移
  - 在主要版本之间的迁移更为简化，体验升级
  - 全新的基于操作的数据复制框架，可加快回复数据
- 性能优化
  - 有效存储稀疏字段的新方法，降低了存储成本
  - 在索引时进行排序，可加快排序的查询性能

# 三、新特性 7.X

- Lucene8.0
- 重大改进 —— 正式废除单个索引下多Type的支持
- 7.1开始，Security功能免费使用
- ECK——Elasticsearch Operator on Kubernetes
- 新功能
  - New Cluster coordination
  - Feature-Complete High Level REST Client
  - Script Score Query
- 性能优化
  - 默认的Primary Shard数从5改为1，避免Over Sharding
  - 性能优化，更快的Top K