# Buff效果-判断数值

# 判断数值-主动 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=<EffectType>
```

以下各项所用的参数都是一样的，区别是包含的算法不同。

|`Effect.Type=<EffectType>`|对应的 AI 脚本动作（数据包的数据结构）|包含的算法|运算目数|
|:-:|:-:|:-:|:-:|
|`Effect.Type=CheckNumber.Eq`|`50400,n`|等于|双目运算|
|`Effect.Type=CheckNumber.NE`|`50401,n`|不等于|双目运算|
|`Effect.Type=CheckNumber.GT`|`50402,n`|大于|双目运算|
|`Effect.Type=CheckNumber.GE`|`50403,n`|大于等于|双目运算|
|`Effect.Type=CheckNumber.LT`|`50404,n`|小于|双目运算|
|`Effect.Type=CheckNumber.LE`|`50405,n`|小于等于|双目运算|

生效时会立刻进行一次【判断】，如果判断成功，则对自己挂载 Buff 并引爆弹头。  
详见 [AI脚本动作-判断数值](/触发与AI脚本动作/AI脚本动作-5-判断数值.md#ai脚本动作-判断数值)。  
`DamageProcessType` 属性无效。

### 效果强度值转换：1+M+N 项参数

|参数|说明|
|:-:|:-|
|【生效概率】|-1 = 0%，-0.2 = 80%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

后续的 M 个参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【挂载持续时间】：  
从第 1 个 Buff 开始（索引 0），单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

后续的 N 个参数项数与 `Effect.Warheads` 的长度相匹配。  
每一项都是【对应每个弹头的伤害】：  
从第 1 个弹头开始（索引 0），0 = 没伤害，负数 = 治疗，默认值是 `0`。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 动画 , 【生效时播放的动画】 , 不写就不显示动画
Effect.DataPack=                                ; 数据包的注册名 , 注意不是填写 IDCode , 数据包含义请见【上述列表给出的 AI 脚本动作】 , 没有数据包直接进入结束状态
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Warheads=                                ; 弹头列表 , 没有弹头就炸不出来
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多投一次 , 默认值是 0 , 单位 : 帧
Effect.Amounts=-1                               ; 整数 , 【引爆的弹头数量】 , 负数 = 全炸一遍 , 0 = 炸不出来 , 默认值是 -1 , 单位 : 个
Effect.Random=no                                ; yes/no , 不全部引爆时 , 是否随机选择弹头 , 不随机则从第一个一直炸到最后一个 , 然后再回到第一个 (永远从第一个弹头开始 , 引爆数量够多可以全炸好几遍) , 默认值是 no
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



# 判断数值-死亡 <亡语>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=<EffectType>
```

以下各项所用的参数都是一样的，区别是包含的算法不同。

|`Effect.Type=<EffectType>`|对应的 AI 脚本动作|包含的算法|运算目数|
|:-:|:-:|:-:|:-:|
|`Effect.Type=CheckNumberDeath.Eq`|`50400,n`|等于|双目运算|
|`Effect.Type=CheckNumberDeath.NE`|`50401,n`|不等于|双目运算|
|`Effect.Type=CheckNumberDeath.GT`|`50402,n`|大于|双目运算|
|`Effect.Type=CheckNumberDeath.GE`|`50403,n`|大于等于|双目运算|
|`Effect.Type=CheckNumberDeath.LT`|`50404,n`|小于|双目运算|
|`Effect.Type=CheckNumberDeath.LE`|`50405,n`|小于等于|双目运算|

生效时会立刻进行一次【判断】，如果判断成功，则对自己挂载 Buff 并引爆弹头。  
详见 [AI脚本动作-判断数值](/触发与AI脚本动作/AI脚本动作-5-判断数值.md#ai脚本动作-判断数值)。  
`DamageProcessType` 属性无效。

### 效果强度值转换：1+M+N 项参数

|参数|说明|
|:-:|:-|
|【生效概率】|-1 = 0%，-0.2 = 80%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

后续的 M 个参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【挂载持续时间】：  
从第 1 个 Buff 开始（索引 0），单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

后续的 N 个参数项数与 `Effect.Warheads` 的长度相匹配。  
每一项都是【对应每个弹头的伤害】：  
从第 1 个弹头开始（索引 0），0 = 没伤害，负数 = 治疗，默认值是 `0`。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 动画 , 【生效时播放的动画】 , 不写就不显示动画
Effect.DataPack=                                ; 数据包的注册名 , 注意不是填写 IDCode , 数据包含义请见【上述列表给出的 AI 脚本动作】 , 没有数据包直接进入结束状态
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Warheads=                                ; 弹头列表 , 没有弹头就炸不出来
Effect.Timer=0                                  ; 整数 , 生效时间的最大值 , 超过时间限制会立刻进入结束状态 , 0 = 无限 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Amounts=-1                               ; 整数 , 【引爆的弹头数量】 , 负数 = 全炸一遍 , 0 = 炸不出来 , 默认值是 -1 , 单位 : 个
Effect.Random=no                                ; yes/no , 不全部引爆时 , 是否随机选择弹头 , 不随机则从第一个一直炸到最后一个 , 然后再回到第一个 (永远从第一个弹头开始 , 引爆数量够多可以全炸好几遍) , 默认值是 no
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。