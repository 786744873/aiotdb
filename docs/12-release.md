## Release版本记录



### 0.1.X计划

- ~~集群化~~
- ~~机器学习~~
- ~~离线计算~~
- ~~事件驱动~~



### 0.1.8 [released]

- SQL 支持 `near` 查询



### 0.1.7 [released]

- 支持 TCP 方式写入



### 0.1.6 [released]

- 支持 Insert Or Replace 操作
- 极致性能优化，磁盘IO占用释放60%，性能提升3倍



### 0.1.5 [released]

- 支持时序数据统计

- 支持时间线节点查询



### 0.1.4 [released]

- 可视化查询
- mqtt设备接入



### 0.1.3 [released]

- 连接池深度优化
- mqtt测点接入
- 批量写入压测
- 通用返回封装



### 0.1.2 [released]

- 启用网络请求压缩，感觉不压缩速度更快
- 支持批量写入
- 支持前后端一体化打包
- 提供`/node`服务器节点状态API
- 添加配置文件，支持动态调整日志等级
- 添加跨域支持



### 0.1.1 [released]

- 添加首页支持
- 提供`/health`健康检查API
- 优化`/query`、`/execute` 接口添加版本号`v1`，变更为`/v1/query`、`/v1/execute`
- 打包使用release生产版本，运行时使用`set GIN_MODE=release`
- 完成同时支持`get`、`post`参数传递



### 0.1.0 [released]

- 开放`/query`、`/execute` 接口
- 数据支持存储与展示
- 打包支持跨平台部署