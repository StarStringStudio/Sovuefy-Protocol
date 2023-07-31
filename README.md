# Sovuefy-Protocol
SovuefyProtocol---一个AI训练数据同步通用协议

## 存在的意义

~~当然是自己用啊~~

在 AI 训练中，很多小白都需要一个类似「WebUI」的玩意来帮助他们

对于 Python 不熟悉或者完全不想用 Python 开发 webui 的开发者往往采用启动 Python 作为子进程的方式来启动训练

而对于如何让训练进程与后端进行完美的沟通，成了一个问题

IPC 是很好的解决方案，吗？

事实上，IPC 在 Python 的实现比较繁琐

那还不如直接输出到 stdout 给 webui 后端捕获呢

## 光速开始
