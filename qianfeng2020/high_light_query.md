# 高亮查询

高亮查询就是你用户输入的关键字，以定的特殊样式展示给用户，让用户知道为什么这个结果被检索出来。

高亮展示的数据，本身就是文档中的一个Field, 单独将Field以highlight的形式返回给你。

ES提供了一个highlight属性， 和query同级别的。

- fragment_size：指定高亮数据展示多少个字符回来。
- pre_tags：指定前缀标签，举个栗子`<font color=" red">`
- post_tags：指定后缀标签，举个栗子`</font>`
- fields：指定哪几个Field以高亮形式返回

```json
# 高亮查询
POST /sms-logs-index/sms-log-type/_search
{
  "query": {
    "match": {
      "smsContent": "盒马"
    }
  },
  "highlight": {
    "fileds": {
      "smsContent": {}
    },
    "pre_tags": "<font color='red'>",
    "post_tags": "</font>",
    "fragment_size": 10
  }
}
```