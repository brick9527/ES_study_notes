# 第八课 Logstash安装与导入数据

# 一、安装

- 下载Logstash（https://www.elastic.co/cn/downloads/past-releases/logstash-7-1-0）
- 解压
- 下载movielen（https://grouplens.org/datasets/movielens/）
- 解压movielen
- 编辑logstash.conf

```sh
input {
  file {
    // 这个path要改成movielen文件的movies.csv文件所在路径  
    path => "/Users/yiruan/dev/elk7/logstash-7.0.1/bin/movies.csv"
    start_position => "beginning"
    sincedb_path => "/dev/null"
  }
}
filter {
  csv {
    separator => ","
    columns => ["id","content","genre"]
  }

  mutate {
    split => { "genre" => "|" }
    remove_field => ["path", "host","@timestamp","message"]
  }

  mutate {

    split => ["content", "("]
    add_field => { "title" => "%{[content][0]}"}
    add_field => { "year" => "%{[content][1]}"}
  }

  mutate {
    convert => {
      "year" => "integer"
    }
    strip => ["title"]
    remove_field => ["path", "host","@timestamp","message","content"]
  }

}
output {
   elasticsearch {
     // 这里可能要换成0.0.0.0  
     hosts => "http://localhost:9200"
     index => "movies"
     document_id => "%{id}"
   }
  stdout {}
}
```

- 运行Logstash

```sh
bin/logstash -f config/logstash.conf // 这个conf文件要是刚才编辑的文件
```

- 运行到不滚动了，就应该是插入完了，ctrl+c结束

