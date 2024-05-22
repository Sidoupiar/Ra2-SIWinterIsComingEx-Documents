# Buff效果-操作作战方局部变量

## 操作作战方局部变量-主动 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=<EffectType>
```

以下各项所用的参数都是一样的，区别是包含的算法不同。

|`Effect.Type=<EffectType>`|对应的 AI 脚本动作（数据包的数据结构）|包含的算法|运算目数|说明|
|:-:|:-:|:-:|:-:|:-|
|`Effect.Type=HouseVar.Add`|`50200,n`|加|双目运算||
|`Effect.Type=HouseVar.Mult`|`50201,n`|乘|双目运算||
|`Effect.Type=HouseVar.Pow`|`50202,n`|指数|双目运算||
|`Effect.Type=HouseVar.Log`|`50203,n`|对数|双目运算||
|`Effect.Type=HouseVar.Mod`|`50204,n`|取模|双目运算||
|`Effect.Type=HouseVar.Eq`|`50210,n`|等于|双目运算||
|`Effect.Type=HouseVar.NE`|`50211,n`|不等于|双目运算||
|`Effect.Type=HouseVar.GT`|`50212,n`|大于|双目运算||
|`Effect.Type=HouseVar.GE`|`50213,n`|大于等于|双目运算||
|`Effect.Type=HouseVar.LT`|`50214,n`|小于|双目运算||
|`Effect.Type=HouseVar.LE`|`50215,n`|小于等于|双目运算||
|`Effect.Type=HouseVar.Not`|`50220,n`|取反|单目运算||
|`Effect.Type=HouseVar.Abs`|`50221,n`|绝对值|单目运算||
|`Effect.Type=HouseVar.Ceil`|`50222,n`|向上取整|单目运算||
|`Effect.Type=HouseVar.Floor`|`50223,n`|向下取整|单目运算||
|`Effect.Type=HouseVar.Round`|`50224,n`|四舍五入|单目运算||
|`Effect.Type=HouseVar.Set`|`50230,n`|覆盖|单目运算||
|`Effect.Type=HouseVar.Max`|`50231,n`|最大|双目运算||
|`Effect.Type=HouseVar.Min`|`50232,n`|最小|双目运算||
|`Effect.Type=HouseVar.Random`|`50233,n`|随机|双目运算|依次为最大值和最小值。|
|`Effect.Type=HouseVar.Delete`|`50234,n`|移除|零目运算||

生效时会立刻进行一次【运算】。  
详见 [AI脚本动作-操作作战方局部变量](/触发与AI脚本动作/AI脚本动作-3-操作作战方局部变量.md#ai脚本动作-操作作战方局部变量)。

### 注意

* 这里使用 `数据包` 中的 `Owner` 而不是 Buff 类型中的。

### 效果强度值转换：1 项参数

|参数|说明|
|:-:|:-|
|【生效概率】|-1 = 0%，-0.2 = 80%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 动画 , 【生效时播放的动画】 , 不写就不显示动画
Effect.DataPack=                                ; 数据包的注册名 , 注意不是填写 IDCode , 数据包含义请见【上述列表给出的 AI 脚本动作】 , 没有数据包直接进入结束状态
Effect.Counts=-1                                ; 整数 , 生效次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧生效一次 , 小于 0 按 0 算 , 但是每一帧最多投一次 , 默认值是 0 , 单位 : 帧
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 操作作战方局部变量-死亡 <亡语>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=<EffectType>
```

以下各项所用的参数都是一样的，区别是包含的算法不同。

|`Effect.Type=<EffectType>`|对应的 AI 脚本动作（数据包的数据结构）|包含的算法|运算目数|说明|
|:-:|:-:|:-:|:-:|:-|
|`Effect.Type=HouseVarDeath.Add`|`50200,n`|加|双目运算||
|`Effect.Type=HouseVarDeath.Mult`|`50201,n`|乘|双目运算||
|`Effect.Type=HouseVarDeath.Pow`|`50202,n`|指数|双目运算||
|`Effect.Type=HouseVarDeath.Log`|`50203,n`|对数|双目运算||
|`Effect.Type=HouseVarDeath.Mod`|`50204,n`|取模|双目运算||
|`Effect.Type=HouseVarDeath.Eq`|`50210,n`|等于|双目运算||
|`Effect.Type=HouseVarDeath.NE`|`50211,n`|不等于|双目运算||
|`Effect.Type=HouseVarDeath.GT`|`50212,n`|大于|双目运算||
|`Effect.Type=HouseVarDeath.GE`|`50213,n`|大于等于|双目运算||
|`Effect.Type=HouseVarDeath.LT`|`50214,n`|小于|双目运算||
|`Effect.Type=HouseVarDeath.LE`|`50215,n`|小于等于|双目运算||
|`Effect.Type=HouseVarDeath.Not`|`50220,n`|取反|单目运算||
|`Effect.Type=HouseVarDeath.Abs`|`50221,n`|绝对值|单目运算||
|`Effect.Type=HouseVarDeath.Ceil`|`50222,n`|向上取整|单目运算||
|`Effect.Type=HouseVarDeath.Floor`|`50223,n`|向下取整|单目运算||
|`Effect.Type=HouseVarDeath.Round`|`50224,n`|四舍五入|单目运算||
|`Effect.Type=HouseVarDeath.Set`|`50230,n`|覆盖|单目运算||
|`Effect.Type=HouseVarDeath.Max`|`50231,n`|最大|双目运算||
|`Effect.Type=HouseVarDeath.Min`|`50232,n`|最小|双目运算||
|`Effect.Type=HouseVarDeath.Random`|`50233,n`|随机|双目运算|依次为最大值和最小值。|
|`Effect.Type=HouseVarDeath.Delete`|`50234,n`|移除|零目运算||

生效时会立刻进行一次【运算】。  
详见 [AI脚本动作-操作作战方局部变量](/触发与AI脚本动作/AI脚本动作-3-操作作战方局部变量.md#ai脚本动作-操作作战方局部变量)。

### 注意

* 这里使用 `数据包` 中的 `Owner` 而不是 Buff 类型中的。

### 效果强度值转换：1 项参数

|参数|说明|
|:-:|:-|
|【生效概率】|-1 = 0%，-0.2 = 80%，0 = 100%，小于 -1 按 -1 算，默认值是 `0`。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 动画 , 【生效时播放的动画】 , 不写就不显示动画
Effect.DataPack=                                ; 数据包的注册名 , 注意不是填写 IDCode , 数据包含义请见【上述列表给出的 AI 脚本动作】 , 没有数据包直接进入结束状态
Effect.Timer=0                                  ; 整数 , 生效时间的最大值 , 超过时间限制会立刻进入结束状态 , 0 = 无限 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 显示相关的作战方局部变量 Buff

详见 [`显示作战方局部变量`](/Buff/Buff效果-7-数值显示.md#显示作战方局部变量-主动)。