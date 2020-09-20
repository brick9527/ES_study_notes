# 第十五课 URI Search详解

# 一、URI Search - 通过URI query实现搜索

```sh
GET /movies/_search?q=2012&df=title&sort=year:desc&from=O&size=10&timeout=1s
{
  "profile": true
}
```

- q:指定查询语句，使用Query String Syntax
- df：默认字段，不指定时，会对所有字段进行查询
- sort：排序,from 和 size用于分页
- profile: 可以查看查询是如何被执行的

# 二、Query String Syntax（1）

- 指定字段 v.s 范查询
  - q=title:2012 / q=2012
  - 指定查询
```sh
#带profile
GET /movies/_search?q=2012&df=title
{
	"profile":"true"
}

GET /movies/_search?q=title:2012
{
	"profile":"true"
}
```
-  - 范查询
```sh
#泛查询，正对_all,所有字段
GET /movies/_search?q=2012
{
	"profile":"true"
}
```

- Term v.s Phrase
  - Beautiful Mind,等效于Beautiful OR Mind
```sh
# Beautiful OR Mind
GET /movies/_search?q=title:Beautiful Mind
{
  "profile":"true"
}
```

 - - "Beautiful Mind",等效于Beautiful AND Mind。phrase查询，还要求前后顺序保持一致

- 分组与引号
  - title:(Beautiful AND Mind)
```sh
#分组，Bool查询
GET /movies/_search?q=title:(Beautiful Mind)
{
	"profile":"true"
}
```

-  - title="Beautiful Mind"

# 三、Query String Syntax（2）

- 布尔操作
  - `AND` / `OR` / `NOT` 或者 `&&` / `||` / `!`
    - 必须大写
    - title:(matrix NOT reloaded)
- 分组
  - `+`表示must
  - `-`表示must_not
  - title:(+matrix -reloaded)

```sh
#布尔操作符
# 查找美丽心灵
GET /movies/_search?q=title:(Beautiful AND Mind)
{
	"profile":"true"
}

# 查找美丽心灵
GET /movies/_search?q=title:(Beautiful NOT Mind)
{
	"profile":"true"
}

# 查找美丽心灵
GET /movies/_search?q=title:(Beautiful %2BMind)
{
	"profile":"true"
}
```

# 四、Query String Syntax（3）

- 范围查询
  - 区间表示: `[]`闭区间，`{}`开区间
  - year:{2019 TO 2018}
  - year:[* TO 2018]
- 算数符号
  - year:>2010
  - year:(>2010 && <=2018)
  - year:(+>2010 +<=2018)

# 五、Query String syntax（4）

- 通配符查询（通配符查询效率低，占用内存大，不建议使用。特别是在最前面）
  - `?`代表1个字符，`*`代表0或多个字符
    - title:mi?d
    - title:be*
- 正则表达式
  - title:[bt]oy
- 模糊匹配与近似查询
  - title:beautiful~1
  - title:"lord rings"~2，不加这个'2'的话，load和rings就要在相邻的位置出现