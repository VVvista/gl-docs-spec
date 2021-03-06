
## 引导
版本管理：兼容性、版本号
错误码：定义使用"."分隔。区分可恢复不可恢复
安全：用户鉴权、数据脱敏
幂等性：防重、幂等
异常：统一错误码、切勿遗传统一 Result

## RPC

## REST

## API GateWay

## 接口规范
### 方法命名
- 新增 以`add`为前缀。例如 `add{XXX}`
- 删除 以`delete`为前缀。例如 `delete{XXX}`
- 修改 以`update`为前缀。例如 `update{XXX}`
- 获取 以`get`为前缀。例如 `get{XXX}`
- 获取 以`get`为前缀、`List`为后缀。例如 `get{XXX}List`
- 保存 以`save`为前缀。例如 `save{XXX}`
- 发送 以`send`为前缀。例如 `send{XXX}`
- 上传 以`upload`为前缀。例如 `upload{XXX}`

### 请求参数
- cookie
- request header： 可以存放 requestId，token,加密字段等
- 请求 body 数据 ：针对入参数据，后端必须做数据验证
- 地址栏参数


### 瘦客户端
客户端任何的修改都是需要发版的，发版需要审核流程。

- 客户端尽量只负责展示逻辑，不处理业务逻辑
- 客户端不处理金额的计算
- 客户端少处理请求参数的校验与约束提示

### 接口日志
方便故障定位，错误重放，日志统计分析等

### 上传/下载
上传/下载，参数增加文件 md5，用于完整性校验

### 性能优化
合并接口
字段简写
无用字段清理
图片裁剪
局部刷新
预加载
其他
接口安全，防参数篡改
频率的控制
数据存储
是否需要依赖于第三方
服务降级，熔断和限流
拆分
扩展性
适配性
幂等
重复提交
部署
缓存穿透、缓存雪崩和缓存击穿
是否需要白名单
预加载
重试
异步
服务端推送或者客户端拉取数据
隔离（例如内网的中台服务，后端服务）
