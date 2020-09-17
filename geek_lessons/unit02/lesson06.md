# 第六课 Kibana的安装与界面快速浏览

# 一、安装kibana

- 下载kibana（https://www.elastic.co/downloads/kibana）
- 解压
- 修改配置

修改`config/kibana.yml`文件，使用原本的localhost不能让虚拟机外的浏览器访问

```sh
server.host: "0.0.0.0"
```

- 运行

运行之前要先启动es

```sh
bin/kibana
```

- 访问5601端口