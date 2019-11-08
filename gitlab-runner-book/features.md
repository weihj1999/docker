# 功能

1. 允许：
  - 多个任务并发运行
  - 多个服务器使用多个token（即使在单个Gitlab项目里面）
  - 每个token允许并发的任务数是有限制的
2. 任务支持不同的运行位置：
  - 本地运行
  - 使用docker容器运行
  - 使用docker容器，通过SSH运行任务
  - 使用docker容器，自动扩展到不同云和虚拟化环境上
  - 连接到一个远程的ssh服务器运行任务
3. 支持Bash， Windows的batch，和PowerShell
4. 可以定制化任务的运行环境
5. 自动加载配置变化，无需重启
6. 支持docker 容器的缓存功能
7. 支持作为服务运行（GNU/Linux， macOS以及Windows）
8. Embedded Prometheus metrics HTTP server
