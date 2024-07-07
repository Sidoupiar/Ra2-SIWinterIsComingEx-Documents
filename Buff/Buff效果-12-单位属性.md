# Buff效果-单位属性

## 加成属性 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=<EffectType>
```

以下各项所用的参数都是一样的，区别是它们操作不同的单位加成属性。

|`Effect.Type=<EffectType>`|操作的属性|备注|
|:-:|:-:|:-|
|`Effect.Type=Prop.MultSpeed`|倍率：移动||
|`Effect.Type=Prop.MultArmor`|倍率：护甲|受到的伤害会`除以`此值，此值为 0 时会强制变为 0.1%。|
|`Effect.Type=Prop.MultVersus`|倍率：护甲伤害|受到的伤害会`乘以`此值。|
|`Effect.Type=Prop.MultAttack`|倍率：火力||
|`Effect.Type=Prop.MultROF`|倍率：射速||
|`Effect.Type=Prop.MultEXP`|倍率：经验值获取|加成变成负数会让单位的经验值越来越少。另见 [获取经验值](/经验值与升级与军衔图像/属性-单位.md#获取经验值)。|
|`Effect.Type=Prop.MultEXPProvide`|倍率：经验值提供|加成变成负数会让单位提供负数的经验值。另见 [提供经验值](/经验值与升级与军衔图像/属性-单位.md#提供经验值)。|
|`Effect.Type=Prop.MultEXPCost`|倍率：经验值获取|`除法`形式的 `MultEXP`，此值为 0 时会强制变为 0.1%。另见 [获取经验值](/经验值与升级与军衔图像/属性-单位.md#获取经验值)。|
|`Effect.Type=Prop.ExMultSpeed`|累乘：移动|由于精度问题，会放弃绝对值小于 1% 的因数。|
|`Effect.Type=Prop.ExMultArmor`|累乘：护甲|由于精度问题，会放弃绝对值小于 1% 的因数。受到的伤害会`除以`此值。|
|`Effect.Type=Prop.ExMultVersus`|累乘：护甲伤害|由于精度问题，会放弃绝对值小于 1% 的因数。受到的伤害会`乘以`此值。|
|`Effect.Type=Prop.ExMultAttack`|累乘：火力|由于精度问题，会放弃绝对值小于 1% 的因数。|
|`Effect.Type=Prop.ExMultROF`|累乘：射速|由于精度问题，会放弃绝对值小于 1% 的因数。|
|`Effect.Type=Prop.ExMultEXP`|累乘：经验值获取|由于精度问题，会放弃绝对值小于 1% 的因数。加成变成负数会让单位的经验值越来越少，另见 [获取经验值](/经验值与升级与军衔图像/属性-单位.md#获取经验值)。|
|`Effect.Type=Prop.ExMultEXPProvide`|累乘：经验值提供|由于精度问题，会放弃绝对值小于 1% 的因数。加成变成负数会让单位提供负数的经验值，另见 [提供经验值](/经验值与升级与军衔图像/属性-单位.md#提供经验值)。|
|`Effect.Type=Prop.ExMultEXPCost`|累乘：经验值获取|由于精度问题，会放弃绝对值小于 1% 的因数。`除法`形式的 `MultEXP`，另见 [获取经验值](/经验值与升级与军衔图像/属性-单位.md#获取经验值)。|
|`Effect.Type=Prop.AddSpeed`|追加：移动||
|`Effect.Type=Prop.AddDamage`|追加：伤害||
|`Effect.Type=Prop.AddAttack`|追加：攻击||
|`Effect.Type=Prop.AddROF`|追加：射速|开火后调整，无法扣成 `0` 延迟。|
|`Effect.Type=Prop.AddEXP`|追加：经验值获取|加成变成负数会让单位的经验值越来越少，另见 [获取经验值](/经验值与升级与军衔图像/属性-单位.md#获取经验值)。|
|`Effect.Type=Prop.AddEXPProvide`|追加：经验值提供|加成变成负数会让单位提供负数的经验值，另见 [提供经验值](/经验值与升级与军衔图像/属性-单位.md#提供经验值)。|
|`Effect.Type=Prop.AddRangeWeapon`|追加：武器射程|因为 Phobos 已经有类似的功能了，并且存在功能冲突，因此这个 Buff 效果种类暂时无效。|
|`Effect.Type=Prop.FinalSpeed`|最终：移动|和追加一样，是加算。|
|`Effect.Type=Prop.FinalDamage`|最终：伤害|和追加一样，是加算。|
|`Effect.Type=Prop.FinalAttack`|最终：攻击|和追加一样，是加算。|
|`Effect.Type=Prop.FinalROF`|最终：射速|和追加一样，是加算。开火后调整，无法扣成 `0` 延迟。|
|`Effect.Type=Prop.FinalEXP`|最终：经验值获取|和追加一样，是加算。加成变成负数会让单位的经验值越来越少，另见 [获取经验值](/经验值与升级与军衔图像/属性-单位.md#获取经验值)。|
|`Effect.Type=Prop.FinalEXPProvide`|最终：经验值提供|和追加一样，是加算。加成变成负数会让单位提供负数的经验值，另见 [提供经验值](/经验值与升级与军衔图像/属性-单位.md#提供经验值)。|
|`Effect.Type=Prop.FinalRangeWeapon`|最终：武器射程|和追加一样，是加算。因为 Phobos 已经有类似的功能了，并且存在功能冲突，因此这个 Buff 效果种类暂时无效。|

属性算法：
```lua
-- Mult   = [倍率]
-- ExMult = [累乘]
-- Add    = [追加]
-- Final  = [最终]
-- 部分 [倍率] 是除法 , [追加] 和 [最终] 均是加法 , 且武器射程暂时无效 , 还请注意
实际值 = ( 原始值 * ( 1.0 + 倍率1 + 倍率2 + ... + 倍率n ) + ( 追加1 + 追加2 + ... + 追加n ) ) * ( 1.0 * 累乘1 * 累乘2 * ... * 累乘n ) + ( 最终1 + 最终2 + ... + 最终n )
```

Buff 会周期性地增加单位加成属性。

### 注意

* 实际的加成均可以是负数，但是 ROF 最终值最低为 1。

* 累乘次数过多可能会因为精度问题导致属性无法完美恢复，因此不建议大量临时属性应用到累乘部分。

### 效果强度值转换：1 项参数

|参数|说明|
|:-:|:-|
|【增加量】|0.2 = 额外增加 20%，1 = 额外增加 100%，默认值是 `0`。|

在此处：  
`Power.Maxs.Total` = 可以达到的最大值（当前 Buff 的总增加量）；  
`Power.Mins.Total` = 可以达到的最小值（当前 Buff 的总增加量）。

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Anims=                                   ; 两个动画 , 【成功增加数值时播放的动画】【成功清除数值时播放的动画】 , 不写就不显示动画
Effect.Counts=-1                                ; 整数 , 增加次数 , 次数耗尽会立刻进入结束状态 , 等于 0 会无法生效并直接进入结束状态 (算作次数耗尽) , 负数 = 无限次 , 默认值是 -1 , 单位 : 次
Effect.Delay=0                                  ; 整数 , 每隔这么多帧增加一次 , 小于 0 按 0 算 , 但是每一帧最多增加一次 , 默认值是 0 , 单位 : 帧
Effect.Modes=0,0                                ; 两个整数 , 【增加模式】【清除模式】 , 无效值默认为 0 , 默认值是 0
                                                ; 【增加模式】
                                                ; 0 = 固定值，不会清除已有的加成，当前 Buff 增加的数值会在 Delay 完毕时刷新
                                                ; 1 = 正常增加
                                                ; 【清除模式】
                                                ; 0 = 进入结束状态时清除当前 Buff 带来的加成
                                                ; 1 = 不做任何处理 (即永久保持加成)
```

### 广播与监听相关属性

不支持广播或监听。

### 数值显示相关属性

不支持数值显示。



## 染色 <主动>

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
Effect.Type=Prop.Tint
```

修改单位的颜色。

### 注意

* 染色不同于颜色，000000 的纯黑实际上是染不上色的。

### 效果强度值转换：3 项参数

|参数|说明|
|:-:|:-|
|【红色】|取值范围 0 - 255，默认值是 `0`。|
|【绿色】|取值范围 0 - 255，默认值是 `0`。|
|【蓝色】|取值范围 0 - 255，默认值是 `0`。|
|【染色强度】|不同来源的染色强度会叠加，基准值是 1000（相当于 Phobos 的 1.0），默认值是 `0`。|

### 效果种类相关属性

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 效果种类相关属性
Effect.Timer=0                                  ; 整数 , 生效时间的最大值 , 超过时间限制会立刻进入结束状态 , 0 = 无限 , 小于 0 按 0 算 , 默认值是 0 , 单位 : 帧
Effect.Owner=All                                ; 作战方归属 , 染色对哪个作战方有效
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
```