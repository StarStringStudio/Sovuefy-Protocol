# 训练部分

训练部分主要分为 step epoch 的更新以及 val 事件的告知

## Step 与 Epoch 的更新

```json
{
    "action":"update_step_epoch",
    "data":{
        "step": 100,
        "epoch":0
    }
}
```

## Val 完成事件

```json
{
    "action":"val_finished",
    "data":{}
}
```