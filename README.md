# micro-todoList
# Go-Micro V2 + RabbitMQ 构造简单备忘录

# 项目的主要功能介绍

- 用户注册登录 ( jwt-go令牌鉴权 )
- 新增/删除/修改/查询 备忘录

# 项目主要依赖：

**Golang**

- Gin
- Gorm
- MySQL
- go-micro
- protobuf
- gRPC
- AMQP(RabbitMQ)
- ini
- hystrix
- jwt-go
- crypto

# 项目结构

## 1. gateway 网关部分

```
gateway/
├── pkg
│  ├── e
│  ├── logging
│  └── util
├── services
│  └── proto
├── weblib
│  ├── handlers
│  └──  middleware
└── wrappers
```
- pkg/e : 封装错误码
- pkg/logging : 日志文件
- pkg/util : 工具函数
- service/proto : 放置proto文件以及生成的pb文件
- weblib/handlers : 各个服务的接口
- weblib/middleware : http服务器的中间件
- wrappers : 放置服务熔断的配置

## 2. mq-server RabbitMQ 消息队列

```
mq-server/
├── conf
├── model
└── service
```

- conf：配置信息
- model：数据库模型
- service：服务

## 3. task & user

```
task/ & user/
├── conf
├── core
├── model
└── service
```

- conf：配置信息
- core：业务逻辑
- model：数据库模型
- service：proto文件以及各服务

conf/config.ini 文件
```ini
[service]
AppMode = debug
HttpPort = :3000

[mysql]
Db = mysql
DbHost = 127.0.0.1
DbPort = 3306
DbUser = root
DbPassWord = root
DbName = micro_todolist

[rabbitmq]
RabbitMQ = amqp
RabbitMQUser = guest
RabbitMQPassWord = guest
RabbitMQHost = localhost
RabbitMQPort = 5672
```



# 运行简要说明
1. 保证rabbitMQ开启状态
2. 保证etcd开启状态
3. 依次执行各模块下的main.go文件
4. 执行user, task, api-gateway的时候需要后面加上这个，注册到etcd并且注册地址是这个地址。
```
go run main.go --registry=etcd --registry_address=127.0.0.1:2379
```

**如果出错一定要注意打开etcd的keeper查看服务是否注册到etcd中。**
