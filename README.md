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

## 目录

1. [训练部分](./train/index.md)

## 数据结构

在 SovuefyProtocol 中，数据是以这样的形式被输出到 stdout 的

```py
sovuefy->{...}
```

`{...}` 当然就是 json 了

这段 json 大概的数据格式如下

```json
{
    "action": "<事件名称>",
    "data": {
        ...
    }
}
```

当然，这段数据会被最小化输出，例如：

```py
sovuefy->{"action":"update_step_epoch","data":{"step":100,"epoch":0}}
```

一个解析 SovuefyProtocol 的 TypeScript 示例如下

```ts
const middleware = (line: string) => {
    if(line.startsWith("sovuefy->")) {
        const data: SovProto = JSON.parse(line.replace("sovuefy->", ""))
        if(data.action == "update_step_epoch") {
            logger.info("Step updated to " + data.data.step)
            logger.info("Epoch updated to " + data.data.epoch)
            /* write your code here */
        }
    }
}
```

这个示例中，假设 middleware 是子进程中的 stdout 的中间件

（实际上传入的是 `Buffer` 需要 `.toString()` 后才能操作）

并且对 step 和 epoch 的更新做出日志输出

# Python 部分

我推荐使用环境变量来确定是否使用 SovuefyProtocol

```py
import os

SOVUEFY_PROTOCOL = not not os.environ['SOVUEFY_PROTOCOL']

global step
global epoch
step = 0
epoch = 0

if __name__ == "__main__":
    # you train code
    if SOVUEFY_PROTOCOL and step % 10 == 0:
        print(f"sovuefy->{json.dumps({
            'action': 'update_step',
            'data': {
                'step': step,
                'epoch': epoch
            }
        })}")
    step += 1
    if step % 114514 == 0:
        epoch += 1
``` 
