# Buff效果-Buff 处理

## 多层效果强度值叠加的 Buff 对目标 Buff 效果强度值的影响

设：

```lua
pow1  = 效果Buff.当前效果强度值
pow2  = 目标Buff.当前效果强度值
base  = 效果Buff.Power.Bases[1]
mult  = 效果Buff.Power.Mults[1]
pmax  = 效果Buff.Power.Maxs[1]
pmin  = 效果Buff.Power.Mins[1]
tmax  = 效果Buff.Power.Maxs.Total[1]
pmax  = 效果Buff.Power.Mins.Total[1]
rmax  = 效果Buff.Power.Maxs.Real[1]
rmin  = 效果Buff.Power.Mins.Real[1]
count = 效果Buff.Effect.Counts
```

则最终结果是：

```lua
pow1 = min( max( pow1 , rmin ) , rmax )
pow2 = pow2 + count * min( max( base + pow1 * mult , pmin ) , pmax )
pow2 = min( max( pow2 , tmin ) , tmax )
```

若 Buff 允许重复激发，则激发时附带的效果强度值会相互叠加，调整 Buff 的效果强度值类效果不受此限制，单纯叠效果强度值并不能激发 Buff，不激发便没有实际效果。



## Buff 挂载 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=BuffMark
```

挂载 Buff 的效果类型，可以影响自己本身，也可以影响周围的单位的 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## Buff 激发 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=BuffActive
```

激发 Buff 的效果类型，可以影响自己本身，也可以影响周围的单位的 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## Buff 结束 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=BuffAfter
```

结束 Buff 的效果类型，可以影响自己本身，也可以影响周围的单位的 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## Buff 移除 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=BuffRemove
```

移除 Buff 的效果类型，可以影响自己本身，也可以影响周围的单位的 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## Buff 强度 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=BuffPower
```

调整 Buff 的效果强度值的效果类型，可以影响自己本身，也可以影响周围的单位的 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## Buff 转换 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=BuffChange
```

转换 Buff 的效果类型，可以影响自己本身，也可以影响周围的单位的 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

* 暂不支持多个 Buff 的转换，也暂不支持进行多次 Buff 的转换。

### 效果强度值转换：4 项参数

|参数|说明|
|:-:|:-|
|【挂载持续时间】|单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，大于 0 则使用此值替换继承的挂载持续时间，负数 = 替换为无限，0 = 不替换，默认值是 `0`，单位：帧。|
|【挂载持续时间增加量】|单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，负数 = 倒扣挂载持续时间，默认值是 `0`，单位：帧。|
|【效果强度值】|单位的 Buff 抗性有效，会改变最终附加的效果强度值，不为 0 则使用此值替换继承的效果强度值，负数效果逆转（不是所有的效果都支持逆转），最终效果取决于 Buff 的效果种类，默认值是 `0`，单位：点。|
|【效果强度值增加量】|单位的 Buff 抗性有效，会改变最终附加的效果强度值，负数 = 倒扣效果强度值，默认值是 `0`，单位：点。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; 两个 Buff , 需要被转换的 Buff , 以及转换后的 Buff , 没有任何一个 Buff 都会无法发挥效果
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否影响单位自己 , 默认值是 yes
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击 Buff 挂载 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackBuffMark
```

攻击有效命中时给攻击者自身挂载 Buff。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击 Buff 激发 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackBuffActive
```

攻击有效命中时激发攻击者自身的 Buff。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击 Buff 结束 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackBuffAfter
```

攻击有效命中时结束攻击者自身的 Buff。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击 Buff 移除 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackBuffRemove
```

攻击有效命中时移除攻击者自身的 Buff。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击 Buff 强度 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackBuffPower
```

攻击有效命中时调整攻击者自身的 Buff 的效果强度值。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击转化 Buff 挂载 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackTransBuffMark
```

攻击有效命中时给攻击者自身挂载 Buff。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的原始伤害转化为【挂载持续时间】，0.8 = 1 点原始伤害转化为 0.8 帧【挂载持续时间】（去尾），负数 = 倒扣【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击转化 Buff 激发 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackTransBuffActive
```

攻击有效命中时激发攻击者自身的 Buff。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的原始伤害转化为【附加的效果强度值】，0.8 = 1 点原始伤害转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 攻击转化 Buff 强度 <攻击>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=AttackTransBuffPower
```

攻击有效命中时调整攻击者自身的 Buff 的效果强度值。

### 注意

* 必须实打实的命中目标单位才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的原始伤害转化为【附加的效果强度值】，0.8 = 1 点原始伤害转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要命中的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御 Buff 挂载 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendBuffMark
```

被命中时给自身挂载 Buff。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御 Buff 激发 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendBuffActive
```

被命中时激发自身的 Buff。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御 Buff 结束 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendBuffAfter
```

被命中时结束自身的 Buff。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御 Buff 移除 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendBuffRemove
```

被命中时移除自身的 Buff。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御 Buff 强度 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendBuffPower
```

被命中时调整自身的 Buff 的效果强度值。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御转化 Buff 挂载 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendTransBuffMark
```

被命中时给自身挂载 Buff。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的原始伤害转化为【挂载持续时间】，0.8 = 1 点原始伤害转化为 0.8 帧【挂载持续时间】（去尾），负数 = 倒扣【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御转化 Buff 激发 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendTransBuffActive
```

被命中时激发自身的 Buff。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的原始伤害转化为【附加的效果强度值】，0.8 = 1 点原始伤害转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 防御转化 Buff 强度 <防御>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DefendTransBuffPower
```

被命中时调整自身的 Buff 的效果强度值。

### 注意

* 必须实打实的被命中才算数，栗如 0 伤武器就有时候无法命中目标单位，这种情况下就不会触发此效果种类。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的原始伤害转化为【附加的效果强度值】，0.8 = 1 点原始伤害转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤 Buff 挂载 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedBuffMark
```

受到伤害和治疗时给自身挂载 Buff。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【伤害类型模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤 Buff 激发 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedBuffActive
```

受到伤害和治疗时激发自身的 Buff。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【伤害类型模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤 Buff 结束 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedBuffAfter
```

受到伤害和治疗时结束自身的 Buff。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【伤害类型模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤 Buff 移除 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedBuffRemove
```

受到伤害和治疗时移除自身的 Buff。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【伤害类型模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤 Buff 强度 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedBuffPower
```

受到伤害和治疗时调整自身的 Buff 的效果强度值。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【伤害类型模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤转化 Buff 挂载 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedTransBuffMark
```

受到伤害和治疗时给自身挂载 Buff。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的实际伤害或治疗转化为【挂载持续时间】，0.8 = 1 点实际伤害或治疗转化为 0.8 帧【挂载持续时间】（去尾），负数 = 倒扣【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【伤害类型模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤转化 Buff 激发 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedTransBuffActive
```

受到伤害和治疗时激发自身的 Buff。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的实际伤害或治疗转化为【附加的效果强度值】，0.8 = 1 点实际伤害或治疗转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【伤害类型模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 受伤转化 Buff 强度 <受伤，治疗>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=DamagedTransBuffPower
```

受到伤害和治疗时调整自身的 Buff 的效果强度值。

### 注意

* 0 伤害不会触发效果。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次攻击造成的实际伤害或治疗转化为【附加的效果强度值】，0.8 = 1 点实际伤害或治疗转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要攻击单位 (如果存在的话) 符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【伤害类型模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【伤害类型模式】
                                                ; 0 = 仅伤害有效
                                                ; 1 = 仅治疗有效
                                                ; 2 = 伤害和治疗均有效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀 Buff 挂载 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillBuffMark
```

击杀单位时给攻击者自身挂载 Buff。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【单位筛选模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀 Buff 激发 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillBuffActive
```

击杀单位时激发攻击者自身的 Buff。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【单位筛选模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀 Buff 结束 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillBuffAfter
```

击杀单位时结束攻击者自身的 Buff。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【单位筛选模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀 Buff 移除 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillBuffRemove
```

击杀单位时移除攻击者自身的 Buff。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：0 项参数

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0                                  ; 整数 , 【单位筛选模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀 Buff 强度 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillBuffPower
```

击杀单位时调整攻击者自身的 Buff 的效果强度值。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【单位筛选模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀转化 Buff 挂载 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillTransBuffMark
```

击杀单位时给攻击者自身挂载 Buff。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次击杀的单位的最大生命值转化为【挂载持续时间】，0.8 = 1 点最大生命值转化为 0.8 帧【挂载持续时间】（去尾），负数 = 倒扣【挂载持续时间】：  
单位的 Buff 抗性有效，会改变最终附加的挂载持续时间，附加后的挂载持续时间会被【实际数值的上下限】限制，默认值是 `0`，单位：帧。

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

在此处：  
`Power.Maxs.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Max` 的值；  
`Power.Mins.Total` 对应项的值，其中 0 = 使用 Buff 的 `Duration.Min` 的值。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【单位筛选模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀转化 Buff 激发 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillTransBuffActive
```

击杀单位时激发攻击者自身的 Buff。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次击杀的单位的最大生命值转化为【附加的效果强度值】，0.8 = 1 点最大生命值转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【单位筛选模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 击杀转化 Buff 强度 <击杀>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=KillTransBuffPower
```

击杀单位时调整攻击者自身的 Buff 的效果强度值。

### 注意

* 必须实际上的击杀才算数，像原地去世 Buff 这种的让单位主动死亡的方式除外。

### 效果强度值转换：N 项参数

参数项数与 `Effect.AcceptBuffs` 的长度相匹配。  
每一项都是【转化率】，把此次击杀的单位的最大生命值转化为【附加的效果强度值】，0.8 = 1 点最大生命值转化为 0.8 点【附加的效果强度值】（去尾），负数 = 倒扣【附加的效果强度值】：  
单位的 Buff 抗性有效，会改变最终附加的效果强度值，附加后的效果强度值会被【实际数值的上下限】限制，负数 = 扣除效果强度值，默认值是 `0`，单位：点。

当没有设置 `Effect.AcceptBuffs` 时，使用第 1 项参数为所有 Buff 提供【附加的效果强度值】。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.Technos=                                 ; 单位列表 , 如果设置了则只有击杀了符合要求的单位才会生效
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每次生效后多长时间内无法再次生效 , 0 = 不限制 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Owner=All                                ; 作战方归属 , 需要被击杀的单位符合作战方
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.Modes=0,0                                ; 两个整数 , 【单位筛选模式】【效果强度值作用模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【单位筛选模式】
                                                ; 设置了 Effect.Technos 后才有效
                                                ; 0 = 击杀了 Effect.Technos 指定的单位才会生效
                                                ; 1 = 击杀了非 Effect.Technos 指定的单位才会生效
                                                ; 【效果强度值作用模式】
                                                ; 0 = 参数项与 Effect.AcceptBuffs 的长度相匹配
                                                ; 1 = 一直使用第 1 项参数为所有 Buff 提供效果
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## Buff 挂载监听 <主动，监听>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=ListenerBuffMark
```

监听指定的广播频道，从而挂载 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

* 出于优化的考量，如果处于主动监听模式，则会把所有频道的广播强度合并处理。

* 如果实际接收到的广播强度为 0，那么将不会发生任何操作。

### 效果强度值转换：2 项参数

|参数|说明|
|:-:|:-|
|【广播强度转化率】|广播强度转化为 Buff 挂载持续时间的比例，0 = 0%，1 = 100%，负数效果翻转，默认值是 `0`。|
|【广播更新后执行操作的概率】|主动监听模式下这是主动执行操作的概率，-1 = 0%，-0.2 = 20%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

根据目标单位是否拥有指定的 Buff，会有不同的细节效果：

1. 如果目标单位没有指定的 Buff，则挂载此 Buff，挂载者为当前 Buff 的持有者，负数 = 永久，0 = 使用 Buff 的默认值。

2. 如果目标单位已有指定的 Buff，则延长此 Buff 的挂载持续时间，负数 = 倒扣挂载持续时间，0 = 没作用。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 三个动画 , 【触发被动监听时播放的动画】【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 挂载这些 Buff , 没设置就不挂载
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 (被动监听模式下 , 0 = 不限制) , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 是否给单位自己挂载 , 默认值是 yes
```

### 广播与监听相关属性

不支持作为广播源。

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 广播与监听相关属性
Listener=no                                     ; yes/no , 开启监听能力 , 默认值是 no
Listener.Channels=0                             ; 整数列表 , 监听的频道 , 写好几个频道就同时监听这几个频道 , 取值范围 : 0 ~ 10000 , 默认值是 0
Listener.Owner=Self                             ; 作战方归属 , 监听这些作战方的广播
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 Self (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Listener.Range=0                                ; 整数 , 需要广播源在这个范围才可以触发监听 , 0 = 不限制 , 默认值是 0 , 单位 : 格子 (开启会比较费性能)
Listener.ActiveMode=yes                         ; yes/no , yes = 主动监听模式 , no = 被动监听模式 , 默认值是 yes (通常来说【被动监听模式】会更费性能 , 请按需选择)
```

### 数值显示相关属性

不支持数值显示。



## Buff 激发监听 <主动，监听>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=ListenerBuffActive
```

监听指定的广播频道，从而激发 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

* 出于优化的考量，如果处于主动监听模式，则会把所有频道的广播强度合并处理。

### 效果强度值转换：2 项参数

|参数|说明|
|:-:|:-|
|【广播强度转化率】|广播强度转化为 Buff 效果强度值的比例，0 = 0%，1 = 100%，负数效果翻转，默认值是 `0`。|
|【广播更新后执行操作的概率】|主动监听模式下这是主动执行操作的概率，-1 = 0%，-0.2 = 20%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 三个动画 , 【触发被动监听时播放的动画】【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 (被动监听模式下 , 0 = 不限制) , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
```

### 广播与监听相关属性

不支持作为广播源。

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 广播与监听相关属性
Listener=no                                     ; yes/no , 开启监听能力 , 默认值是 no
Listener.Channels=0                             ; 整数列表 , 监听的频道 , 写好几个频道就同时监听这几个频道 , 取值范围 : 0 ~ 10000 , 默认值是 0
Listener.Owner=Self                             ; 作战方归属 , 监听这些作战方的广播
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 Self (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Listener.Range=0                                ; 整数 , 需要广播源在这个范围才可以触发监听 , 0 = 不限制 , 默认值是 0 , 单位 : 格子 (开启会比较费性能)
Listener.ActiveMode=yes                         ; yes/no , yes = 主动监听模式 , no = 被动监听模式 , 默认值是 yes (通常来说【被动监听模式】会更费性能 , 请按需选择)
```

### 数值显示相关属性

不支持数值显示。



## Buff 结束监听 <主动，监听>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=ListenerBuffAfter
```

监听指定的广播频道，从而结束 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

* 如果实际接收到的广播强度为 0，那么将不会发生任何操作。

### 效果强度值转换：1 项参数

|参数|说明|
|:-:|:-|
|【广播更新后执行操作的概率】|主动监听模式下这是主动执行操作的概率，-1 = 0%，-0.2 = 20%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 三个动画 , 【触发被动监听时播放的动画】【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 (被动监听模式下 , 0 = 不限制) , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
```

### 广播与监听相关属性

不支持作为广播源。

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 广播与监听相关属性
Listener=no                                     ; yes/no , 开启监听能力 , 默认值是 no
Listener.Channels=0                             ; 整数列表 , 监听的频道 , 写好几个频道就同时监听这几个频道 , 取值范围 : 0 ~ 10000 , 默认值是 0
Listener.Owner=Self                             ; 作战方归属 , 监听这些作战方的广播
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 Self (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Listener.Range=0                                ; 整数 , 需要广播源在这个范围才可以触发监听 , 0 = 不限制 , 默认值是 0 , 单位 : 格子 (开启会比较费性能)
Listener.ActiveMode=yes                         ; yes/no , yes = 主动监听模式 , no = 被动监听模式 , 默认值是 yes (通常来说【被动监听模式】会更费性能 , 请按需选择)
```

### 数值显示相关属性

不支持数值显示。



## Buff 移除监听 <主动，监听>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=ListenerBuffRemove
```

监听指定的广播频道，从而移除 Buff。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

* 如果实际接收到的广播强度为 0，那么将不会发生任何操作。

### 效果强度值转换：1 项参数

|参数|说明|
|:-:|:-|
|【广播更新后执行操作的概率】|主动监听模式下这是主动执行操作的概率，-1 = 0%，-0.2 = 20%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 三个动画 , 【触发被动监听时播放的动画】【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 (被动监听模式下 , 0 = 不限制) , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
```

### 广播与监听相关属性

不支持作为广播源。

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 广播与监听相关属性
Listener=no                                     ; yes/no , 开启监听能力 , 默认值是 no
Listener.Channels=0                             ; 整数列表 , 监听的频道 , 写好几个频道就同时监听这几个频道 , 取值范围 : 0 ~ 10000 , 默认值是 0
Listener.Owner=Self                             ; 作战方归属 , 监听这些作战方的广播
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 Self (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Listener.Range=0                                ; 整数 , 需要广播源在这个范围才可以触发监听 , 0 = 不限制 , 默认值是 0 , 单位 : 格子 (开启会比较费性能)
Listener.ActiveMode=yes                         ; yes/no , yes = 主动监听模式 , no = 被动监听模式 , 默认值是 yes (通常来说【被动监听模式】会更费性能 , 请按需选择)
```

### 数值显示相关属性

不支持数值显示。



## Buff 强度监听 <主动，监听>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=ListenerBuffPower
```

监听指定的广播频道，从而调整 Buff 的效果强度值。

### 注意

* 范围不要搞得太大，更不要全图，它会卡的。

* 出于优化的考量，如果处于主动监听模式，则会把所有频道的广播强度合并处理。

* 如果实际接收到的广播强度为 0，那么将不会发生任何操作。

### 效果强度值转换：2 项参数

|参数|说明|
|:-:|:-|
|【广播强度转化率】|广播强度转化为 Buff 效果强度值的比例，0 = 0%，1 = 100%，负数效果翻转，默认值是 `0`。|
|【广播更新后执行操作的概率】|主动监听模式下这是主动执行操作的概率，-1 = 0%，-0.2 = 20%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 三个动画 , 【触发被动监听时播放的动画】【生效时在自己身上播放的动画】【生效时在受影响单位身上播放的动画】 , 不写就不显示动画
Effect.AcceptBuffs=                             ; Buff 列表 , 仅对这些 Buff 生效 , 不写或留空表示允许任意 Buff
Effect.ExceptBuffs=                             ; Buff 列表 , 不对这些 Buff 生效 , 如果两个列表都设置了就必须同时满足两个列表才行
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多生效一次 (被动监听模式下 , 0 = 不限制) , 默认值是 0 , 单位 : 帧
Effect.Range=0                                  ; 浮点数 , 影响的范围 (半径) , 0 = 只影响挂载了当前 Buff 的单位 , 大于 0 会影响范围内的单位的 Buff , 小于 0 按 0 算 , 默认值是 0 , 单位 : 格子
Effect.Self=yes                                 ; yes/no , 影响当前 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 影响其他 Buff (挂载了当前 Buff 的单位) , 默认值是 yes
```

### 广播与监听相关属性

不支持作为广播源。

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 广播与监听相关属性
Listener=no                                     ; yes/no , 开启监听能力 , 默认值是 no
Listener.Channels=0                             ; 整数列表 , 监听的频道 , 写好几个频道就同时监听这几个频道 , 取值范围 : 0 ~ 10000 , 默认值是 0
Listener.Owner=Self                             ; 作战方归属 , 监听这些作战方的广播
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 Self (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Listener.Range=0                                ; 整数 , 需要广播源在这个范围才可以触发监听 , 0 = 不限制 , 默认值是 0 , 单位 : 格子 (开启会比较费性能)
Listener.ActiveMode=yes                         ; yes/no , yes = 主动监听模式 , no = 被动监听模式 , 默认值是 yes (通常来说【被动监听模式】会更费性能 , 请按需选择)
```

### 数值显示相关属性

不支持数值显示。