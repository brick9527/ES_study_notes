# 基于词项和基于全文的搜索

# 基于Term的查询

- Term的重要性
  - **Term是表达语义的最小单位**。搜索和利用统计语言模型进行自然语言处理都需要处理term
- 特点
  - Term level Query：Term Query / Range Query / Exists Query / Prefix Query / Wildcard Query
  - 在ES中，Term查询，对输入不做分词。会将输入作为一个整体，在倒排索引中查找准确的词项，并且使用相关度算分公式为每个包含该词项的文档进行**相关度算分**。例如“Apple Store”
  - 可以通过Constant Score将查询转换成一个Filtering，避免算分，并利用缓存，提高性能

# 复合查询

- 将Query转成Filter，忽略TF-IDF计算，避免相关性算分的开销
- Filter可以有效利用缓存

# 基于全文的查询

- 基于全文的查找
  - Match Query / Match phrase Query / Query String Query
- 特点
  - 索引和搜索时都会进行分词，查询字符串先传递到一个合适的分词器，然后生成一个供查询的词项列表
  - 查询时候，先会对输入的查询进行分词，然后每个词项逐个进行底层的查询，最终将结果进行合并。并为每个文档生成一个算分。例如查“Matrix reloaded”，会查到包括Matrix或者reload的所有结果