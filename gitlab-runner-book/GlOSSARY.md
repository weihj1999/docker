# Git
分布式版本管理工具

# MarkDown
普通文本转换标记语言的语法

# JavaScript
运行在浏览器的脚本

# Node.js
Node.js 是一个基于Chrome JavaScript 运行的一个平台， 用来方便地搭建快速的、易于扩展的网络应用。Node.js 借助事件驱动， 非阻塞 I/O 模型变得轻量和高效，非常适合 运行密集型实时在线应用。

# Gitlab-runner
Gitlab 执行各种CI，CD任务，并返回结果到Gitlab实例的执行机。

# 构建执行机
一个运行gitlab-runner的执行机，用于执行抓包，打包，传包，测试，部署的runner环境。经常使用到的构建执行机有
- docker执行机
- rpm执行机
- tar压缩执行机
- 其他

# 主执行机
用于创建构建执行机，并自动注册构建执行机到gitlab中心，主执行机上储备有不同类型的配置文件用于创建不同类型的构建执行机。
