# 第五课 ES的安装与简单配置

# 一、安装JAVA

- 运行ES，需要安装并配置JDK
  - 设置$JAVA_HOME
- 各个版本对Java的依赖
- ES5需要Java 8以上版本
- ES从6.5开始支持Java 11
- https://www.elastic.co/support/matrix#matrix_jvm
- 7.0开始，内置了Java环境

# 二、ES的文件目录结构

|目录|配置文件|描述|
|-|-|-|
|bin||脚本文件，包括启动ES，安装插件。运行统计数据等|
|config|elasticsearch.yml|集群配置文件，user、role based相关配置|
|JDK||Java运行环境|
|data|path.data|数据文件|
|lib||Java类库|
|logs|path.log|日志文件|
|modules||包含所有ES模块|
|plugins||包含所有已安装插件|

# 三、JVM配置

- 修改JVM-config / jvm.options
  - 7.1 下载的默认设置是1GB
- 配置的建议
  - Xmx和Xms设置成一样
  - Xmx不要超过机器内存的50%
  - 不要超过30GB

# 四、安装插件

- 查看插件列表
  
```sh
elasticsearch-plugin list
```

- 安装插件

```sh
elasticsearch-plugin install analysis-icu
```

# 五、运行多个ES实例构成集群

```sh
elasticsearch -E node.name=node1 -E cluster.name=geektime -E path.data=node1_data -d
elasticsearch -E node.name=node2 -E cluster.name=geektime -E path.data=node2_data -d
elasticsearch -E node.name=node3 -E cluster.name=geektime -E path.data=node3_data -d
```

浏览器访问`localhost:9200/_cat/nodes`查看集群

# 六、安装记录

1. 下载es7.1.0，解压
2. 修改配置文件
```sh
node.name: node-1

network.host: 0.0.0.0 // 必须这个，不然外网无法访问

cluster.initial_master_nodes: ["node-1"] // 这个不设置要报错
```

3. 修改系统配置

`/etc/sysctl.conf`最后一行加上：

```sh
vm.max_map_count=262144
```

4. 启动报错

`can not run elasticsearch as root`

解决方法：

- 创建新用户

```sh
adduser [用户名]
```

然后给es的文件夹分配权限

```sh
chown -R [用户名] ./elasticsearch-7.1
```