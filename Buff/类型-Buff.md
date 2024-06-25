# 类型-Buff

## 注册

位于 `rulesmd.ini`：

```ini
[BuffTypes]
0=BuffType1
1=BuffType2
2=BuffType3
```

### 注意

* 索引从 0 开始。

* 此类型并不需要注册，只是提供了这个功能。  
如果注册的话，建议在 `rulesmd.ini` 本体中注册。  
如果遇到 Buff 不发挥效果的情况，并且确定是没有载入的话，请反馈。



## 完整结构

位于 `rulesmd.ini`：

```ini
[SomeBuffType]
; 基础属性
Order=0                                         ; 整数 , Buff 的处理顺序 , 值越小越靠前 , 值相同则顺序取决于挂载的时间 , 挂载越早越靠前 , 可以是负数 , 默认值是 0
                                                ; 在处理 Buff 的生效效果的过程中 , 仅当 Buff 的处理阶段类型与当前处理阶段相匹配时 (<> 中的字段 , 逗号分隔) , 处理顺序才有效 , Buff 仅在列出的处理阶段中才会参与处理流程
AllowExist=yes,yes,yes,yes                      ; 四个 yes/no , Buff 在单位【转换】【进入载具】【进入建筑驻军】【单位断电、被 EMP、被静默等】时是否保留 (均无法恢复) , 不写或留空使用默认值 , 默认值是 yes,yes,yes
                                                ; 【转换】包括展开或收起 , 还有类似多段升级的这种 , 暂不支持武直这种类型的转换
                                                ; 【单位断电、被 EMP、被静默等】中 , 被静默需要分模式 , 仅建筑的模式 1 和 2 , 以及单位的模式 1 会被读取
AllowPlayer=yes                                 ; yes/no , 是否允许玩家持有的单位挂载此 Buff , 默认值是 yes
AllowAI=yes                                     ; yes/no , 是否允许 AI 持有的单位挂载此 Buff , 中立阵营也算 AI 控制的 , 默认值是 yes
AllowAIDifficulty=yes,yes,yes                   ; 三个 yes/no , 是否允许 AI 持有的单位挂载此Buff , 按难度区分 , 分别是【简单】【中等】【冷酷】 , 默认值是 yes,yes,yes
AllowGameMode=yes,yes                           ; 两个 yes/no , 是否允许在战役模式和其他模式 (遭遇战) 下挂载此 Buff , 默认值是 yes,yes
EnableCodeDamage=yes                            ; yes/no , Buff 是否会忽视 (不处理) 拥有 IgnoreDefenses=yes 标识的伤害 (通常是各种机制杀的伤害) , 当 Buff 拥有伤害处理相关的处理阶段时才有效 , 亡语不受此控制 (效果会正常触发) , yes = 忽视这样的伤害 , 默认值是 yes
NeedPowered=no                                  ; yes/no , Buff 是否会在单位断电、被 EMP、被静默等时失效 , 不影响下方的各种动画 , 默认值是 no
                                                ; 其中被静默需要分模式 , 仅建筑的模式 1 和 2 , 以及单位的模式 1 会被读取
WaitingIronCurtain=no                           ; yes/no , Buff 是否会在单位处于铁幕效果下时失效 , 铁幕消失后恢复效果 , 不影响下方的各种动画 , 默认值是 no
                                                ; yes = 当单位处于铁幕效果下时 , Buff 进入生效状态后会被冻结 , 效果不会生效 , 冷却时间不恢复 , 弹头不炸 , 伤害不处理 , 亡语不触发等等
LockOwnerHouse=no                               ; yes/no , 作战方是否固定为挂载瞬间的作战方 , yes = 维持挂载瞬间的作战方 , no = 当单位被心控时作战方也会变化 , 默认值是 no
                                                ; 这里有个特殊情况 , 当挂载瞬间的作战方被击败后 , 此项强制为 no
DamageProcessType=Detonate                      ; 伤害处理类型 , 对于可以造成伤害的 Buff 效果 , 是否造成真正的攻击伤害 , 不管如何设置都需要指定弹头 , 默认值是 Detonate (不区分大小写)
                                                ; 可用值 : Detonate , Damage , Health , OnlyHealth
                                                ; Detonate   = 引爆弹头造成伤害 , 和被攻击命中一样
                                                ; Damage     = 不引爆弹头直接传递伤害 , 弹头的 CellSpread 和动画等需要抛射体的效果会无效 , 其他效果理论上正常
                                                ; Health     = 直接扣除目标的生命值 , 而非造成实际的攻击伤害 , 此处理不做受击结算 , 不算做当前 Buff 所属单位的攻击 , 不吃火力和防御等的加成
                                                ;              仅执行目标单位的 Buff 的【自身处理阶段】 , 可以把目标扣死 , 届时会产生 1 真伤的死亡伤害 , 此伤害可以被免疫导致死不成
                                                ;              此属性对恢复和即死等效果造成的死亡伤害无效 , 对发射武器和 (检测) 条件引爆弹头等效果造成的间接伤害无效
                                                ; OnlyHealth = 类似于 Health , 但是不执行目标单位的 Buff 的【自身处理阶段】

; 动画相关属性
; 以下动画均需要注册 , 不支持 Bouncer=yes 的动画
Anim=None,None,None,None                        ; 四个动画 , 通常情况下显示的动画 , 对应【挂载】【激发】【生效】【结束】四个状态 , 不写或留空使用默认值 , 默认值是 None (即无动画 , 不区分大小写)
Anim.Death=None,None,None,None                  ; 四个动画 , 单位死亡显示的动画 , 对应【挂载】【激发】【生效】【结束】四个状态 , 不写或留空使用默认值 , 默认值是 None (即无动画 , 不区分大小写)
Anim.Removed=None,None,None,None                ; 四个动画 , Buff 被移除时显示的动画 , 对应【挂载】【激发】【生效】【结束】四个状态 , 不写或留空使用默认值 , 默认值是 None (即无动画 , 不区分大小写)
Anim.End=None,None,None,None                    ; 四个动画 , Buff 挂载持续时间结束时显示的动画 , 对应【挂载】【激发】【生效】【结束】四个状态 , 不写或留空使用默认值 , 默认值是 None (即无动画 , 不区分大小写)
Anim.BuffDeath=None,None,None,None              ; 四个动画 , Buff 生命值清零时显示的动画 , 对应【挂载】【激发】【生效】【结束】四个状态 , 不写或留空使用默认值 , 默认值是 None (即无动画 , 不区分大小写)

; 挂载持续时间相关属性
Duration=-1                                     ; 整数 , 默认的 Buff 的挂载持续时间 , 仅当挂载 Buff 时输入的时间为 0 时生效 , 负数 = 永久 , 0 = 挂不上 , 默认值是 -1 , 单位 : 帧
                                                ; 对于非永久的 Buff , 挂载持续时间的硬上限是 10e 帧 , 无法超过这个值 , 挂载持续时间在 10e 的范围内可以一直叠加 , 但是无法叠加到【无限】这个状态
Duration.Max=None                               ; 整数 , 每次挂载 Buff 时 (叠加挂载持续时间时) 允许的挂载持续时间上限 , 负数或 None 或 N = 不限制 , 默认值是 None (不区分大小写) , 单位 : 帧
Duration.Min=None                               ; 整数 , 每次挂载 Buff 时 (叠加挂载持续时间时) 允许的挂载持续时间下限 , 负数或 None 或 N = 不限制 , 默认值是 None (不区分大小写) , 单位 : 帧
                                                ; 上下限仅对非永久的 Buff 有效

; 生命值相关属性
Health=-1                                       ; 浮点数 , 默认的 Buff 的生命值 , 仅当挂载 Buff 时输入的生命值为 0 时生效 , 负数 = 无敌 , 0 = 一碰就凉 , 默认值是 -1 , 单位 : 点
                                                ; 对于非无敌的 Buff , 生命值的硬上限是 10e 点 , 无法超过这个值 , 生命值在 10e 的范围内可以一直恢复 , 但是无法恢复到【无敌】这个状态
Health.Max=None                                 ; 浮点数 , 调整 Buff 的生命值时 (恢复生命值时) 允许的生命值上限 , 负数或 None 或 N = 不限制 , 默认值是 None (不区分大小写) , 单位 : 点
Health.Min=None                                 ; 浮点数 , 调整 Buff 的生命值时 (恢复生命值时) 允许的生命值下限 , 负数或 None 或 N = 不限制 , 默认值是 None (不区分大小写) , 单位 : 点
                                                ; 上下限仅对非无敌 Buff 有效
Health.Damage=no                                ; yes/no , 单位受到伤害时 Buff 是否也会被伤害 , 默认值是 no
                                                ; Buff 使用单位的护甲 , 且在受伤阶段处理完毕后才会结算生命值 , Buff 生命值清零时会被立刻移除

; 状态控制相关属性
Mark.Multy=yes                                  ; yes/no , 挂载期间是否允许重复挂载
Active.Auto=-1                                  ; 整数 , 获取 Buff 后经过多久就会自动激发 , 小于 0 视为不自动激发 , 0 = 立即进入激发状态 , 默认值是 [BuffControls] -> Default.Active.Auto 的值 , 单位 : 帧
Active.Auto.Set=                                ; Buff 参数设置 , 自动激发时会合并此设置
Active.Auto.Check=                              ; 判断单位条件 , 条件判断 , 需要满足所有的条件才会自动激发 , 默认值是 空
                                                ; 满足 Active.Auto 之后才会判断 , Active.Auto=0 时在挂载后会立刻判断一次
                                                ; 判断时来源单位是 Buff 的来源单位 , 目标单位是被挂载 Buff 的单位自己
Active.Auto.CheckDelay=31                       ; 整数 , 每次判断的时间间隔 ， 负数和 0 和 1 = 每帧检测一次 (注意性能) , 默认值是 31
Active.Before=0                                 ; 整数 , 挂载 Buff 后多久才允许激发 , 小于 0 视为 0 处理 , 默认值是 0 , 单位 : 帧
Active.After=0                                  ; 整数 , 激发结束后多久才允许重复激发 , 小于 0 视为 0 处理 , 默认值是 0 , 单位 : 帧
Active.Delay=0                                  ; 整数 , 激发后多久才会生效 , 生效前 Buff 过期的话就没有效果了 , 0 = 立即进入生效状态 , 小于 0 视为 0 处理 , 默认值是 0 , 单位 : 帧
Active.Multy=no                                 ; yes/no , 激发期间是否允许重复激发
                                                ; 若 Buff 允许重复激发 , 则激发时附带的效果强度值会相互叠加 , 调整 Buff 的效果强度值类效果不受此限制 , 单纯叠效果强度值不能激发 Buff , 不激发便没有实际效果
After.Type=After                                ; 准备进入结束状态时 , Buff 何去何从 (具体激发时长取决于效果种类 , 不区分大小写)
                                                ; After     = 变成结束状态 , 默认值是 After
                                                ; Reset     = 变成挂载状态 , 可以重新激发 , 经过 1 帧后才会切换 (硬限制)
                                                ; Remove    = 移除此 Buff
                                                ; Next      = 切换至新的 Buff (旧的会被移除)
                                                ; AfterNext = 切换至新的 Buff (旧的会被移除) , 但是是在挂载持续时间结束之后
After.NextBuffs=                                ; Buff 列表 , 当【After.Type=Next】或【After.Type=AfterNext】时要切换为这些 Buff , 如果为空则效果等同于 Remove
                                                ; 若填写当前 Buff 类型则等同于没填写 (即无法递归挂载 , 因为挂载持续时间会叠加而非挂载一个新的同类 Buff)
After.NextBuff.Sets=                            ; Buff 参数设置列表 , 切换的 Buff 在挂载时会合并这些设置 , 顺序与 After.NextBuffs 相对应 , 不设置则使用Buff 的默认值
                                                ; 当【After.Type=Next】时 , Buff 参数设置中的 Duration = 0 表示继承当前 Buff 的挂载持续时间 (不设置则强制继承当前 Buff 的挂载持续时间)

; 效果种类相关属性
Effect.Type=None                                ; 生效时的效果种类 (只能填写一个) , 不同种类效果不同 , 具体见下 (不区分大小写) , 默认值是 None

; 以下属性是 Buff 所支持的 , 但是具体作用取决于 Effect.Type 的值 , 没提到的属性意味着在对应的效果种类中无效
Effect.AcceptBuffs=                             ; Buff 列表
Effect.ExceptBuffs=                             ; Buff 列表
Effect.Technos=                                 ; 单位列表
Effect.Weapons=                                 ; 武器列表
Effect.Warheads=                                ; 弹头列表
Effect.Anims=                                   ; 动画列表 , 需要注册
Effect.AnimsOthers=                             ; 动画列表 , 需要注册
Effect.DataPack=                                ; 数据包的注册名 , 注意不是填写 IDCode
Effect.Damages=1                                ; 整数列表 , 默认值是 1
Effect.Counts=-1                                ; 整数列表 , 默认值是 -1
Effect.Amounts=-1                               ; 整数列表 , 默认值是 -1
Effect.AnimDelays=31                            ; 整数列表 , 默认值是 31
Effect.Modes=0                                  ; 整数列表 , 一般用于确定 Buff 的行为模式 , 无效值默认为 0 , 默认值是 0
Effect.Values=0                                 ; 浮点数列表 , 默认值是 0
Effect.Healths=-1                               ; 浮点数列表 , 默认值是 -1
Effect.Power.Limits=-1                          ; 浮点数列表 , 默认值是 -1
Effect.UnitType=All                             ; 单位种类
                                                ; 可用值 : All (无简写) , Building | B , Infantry | I , Unit | U , Aircraft | A , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多个单位种类时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配建筑和载具 : Building,Unit 或 B,U (简写可以混用 , 不要有空格)
Effect.Owner=All                                ; 作战方归属
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 All (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Effect.ExtraCodeA=0                             ; 整数 , 额外代码 A , 与 Effect.Modes 不同 , 它是支持动态修改的 , 无效值默认为 0 , 默认值是 0
Effect.ExtraCodeB=0                             ; 整数 , 额外代码 B , 与 Effect.Modes 不同 , 它是支持动态修改的 , 无效值默认为 0 , 默认值是 0
Effect.Timer=0                                  ; 整数 , 小于 0 按 0 算 , 默认值是 0
Effect.Delay=0                                  ; 整数 , 小于 0 按 0 算 , 默认值是 0
Effect.Range=0                                  ; 浮点数 , 一般表示范围 , 小于 0 按 0 算 , 默认值是 0
Effect.Random=no                                ; yes/no , 一般表示是否随机 , 默认值是 no
Effect.Display=yes                              ; yes/no , 一般用于是否显示提示信息 (仅游戏原本的行为) , 默认值是 yes
Effect.Self=yes                                 ; yes/no , 一般表示是否影响自己 (单位) , 默认值是 yes
Effect.Other=yes                                ; yes/no , 一般表示是否影响其他 (单位) , 默认值是 yes
Effect.Attacker=no                              ; yes/no , 一般表示是否影响攻击者 (单位 , 当前单位是被攻击者) , 默认值是 no
Effect.Source=no                                ; yes/no , 一般表示是否影响来源 (单位) , 默认值是 no
Effect.Target=no                                ; yes/no , 一般表示是否影响目标 (单位) , 默认值是 no
Effect.FromSource=no                            ; yes/no , 默认值是 no
Effect.TargetSource=no                          ; yes/no , 默认值是 no
Effect.OffsetSource=0,0,0                       ; 三个整数 , 用于坐标或偏移量 , 默认值是 0,0,0
Effect.OffsetSourceBase=0,0,0                   ; 三个整数 , 用于坐标或偏移量 , 默认值是 0,0,0
Effect.OffsetTarget=0,0,0                       ; 三个整数 , 用于坐标或偏移量 , 默认值是 0,0,0
Effect.OffsetTargetBase=0,0,0                   ; 三个整数 , 用于坐标或偏移量 , 默认值是 0,0,0
Effect.OffsetSourceMotion=                      ; 坐标运动轨迹 , Effect.OffsetSource 的运动轨迹 (性能略微低一点) , 空 = 禁用 , 默认值是 空
Effect.OffsetSourceBaseMotion=                  ; 坐标运动轨迹 , Effect.OffsetSourceBase 的运动轨迹 (性能略微低一点) , 空 = 禁用 , 默认值是 空
Effect.OffsetTargetMotion=                      ; 坐标运动轨迹 , Effect.OffsetTarget 的运动轨迹 (性能略微低一点) , 空 = 禁用 , 默认值是 空
Effect.OffsetTargetBaseMotion=                  ; 坐标运动轨迹 , Effect.OffsetTargetBase 的运动轨迹 (性能略微低一点) , 空 = 禁用 , 默认值是 空
Effect.OffsetSourceDirection=Self               ; 跟随旋转类型 , 用于决定 Effect.OffsetSource 如何跟随旋转
                                                ; 可用值 : Self = 实际朝向 , Body = 身体朝向 , Target = 自身 -> 目标的方向作为朝向 , Random = 随机朝向 , None = 不跟随旋转 , 默认值是 Self (不区分大小写)
Effect.OffsetSourceBaseDirection=Self           ; 跟随旋转类型 , 用于决定 Effect.OffsetSourceBase 如何跟随旋转
                                                ; 可用值 : Self = 实际朝向 , Body = 身体朝向 , Target = 自身 -> 目标的方向作为朝向 , Random = 随机朝向 , None = 不跟随旋转 , 默认值是 Self (不区分大小写)
Effect.OffsetTargetDirection=Self               ; 跟随旋转类型 , 用于决定 Effect.OffsetTarget 如何跟随旋转 , 默认值是 Self (不区分大小写)
                                                ; 可用值 : Self = 实际朝向 , Body = 身体朝向 , Target = 自身 -> 目标的方向作为朝向 , Random = 随机朝向 , None = 不跟随旋转 , Same = 使用 Effect.OffsetSourceDirection 的值 , 默认值是 Self (不区分大小写)
Effect.OffsetTargetBaseDirection=Self           ; 跟随旋转类型 , 用于决定 Effect.OffsetTargetBase 如何跟随旋转
                                                ; 可用值 : Self = 实际朝向 , Body = 身体朝向 , Target = 自身 -> 目标的方向作为朝向 , Random = 随机朝向 , None = 不跟随旋转 , Same = 使用 Effect.OffsetSourceBaseDirection 的值 , 默认值是 Self (不区分大小写)

; 效果强度值相关属性
; 需要注意的是 , 这里的属性是用于定义【效果强度值】如何转化为【实际效果】的 , Buff 的实际上的【效果强度值】由【Buff 参数设置】中的 Power 属性提供
Power.Bases=0                                   ; 浮点数列表 , 实际效果的基础数值 , 默认值是 0
Power.Mults=0                                   ; 浮点数列表 , 效果强度值的转换效率 , 默认值是 0
Power.Maxs=None                                 ; 浮点数列表 , 效果的数值上限 , None 或 N = 不限制 , 默认值是 None (不区分大小写)
Power.Mins=None                                 ; 浮点数列表 , 效果的数值下限 , None 或 N = 不限制 , 默认值是 None (不区分大小写)
Power.Maxs.Total=None                           ; 浮点数列表 , 实际数值的数值上限 (【实际数值的上下限】效果取决于效果种类的具体参数项) , None 或 N = 不限制 , 默认值是 None (不区分大小写)
Power.Mins.Total=None                           ; 浮点数列表 , 实际数值的数值下限 (【实际数值的上下限】效果取决于效果种类的具体参数项) , None 或 N = 不限制 , 默认值是 None (不区分大小写)
Power.Maxs.Real=None                            ; 浮点数列表 , 在效果强度值转换的过程中参与计算的效果强度值上限 , None 或 N = 不限制 , 默认值是 None (不区分大小写)
Power.Mins.Real=None                            ; 浮点数列表 , 在效果强度值转换的过程中参与计算的效果强度值下限 , None 或 N = 不限制 , 默认值是 None (不区分大小写)
Power.Maxs.Effect=None                          ; 浮点数列表 , 生效区间上限 , 只有效果强度值小于等于此值时才参与计算 , None 或 N = 不限制 , 默认值时 None (不区分大小写)
Power.Mins.Effect=None                          ; 浮点数列表 , 生效区间下限 , 只有效果强度值大于等于此值时才参与计算 , None 或 N = 不限制 , 默认值时 None (不区分大小写)

; 广播与监听相关属性
; 需要 Buff 处于生效阶段 , 但是并不是所有的效果种类都支持广播与监听
Broadcast=no                                    ; yes/no , 开启广播能力 , 默认值是 no
Broadcast.Channels=0                            ; 整数列表 , 广播的频道 , 写好几个频道就在好几个频道上发布广播更新 , 取值范围 : 0 ~ 10000 , 默认值是 0
Broadcast.Owner=Self                            ; 作战方归属 , 在这些作战方的频道上发布广播更新
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 Self (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Broadcast.Fresh=yes                             ; yes/no , 可以控制是否发布广播更新 , yes = 发布广播更新 , no = 仅同步广播强度 , 不发布广播更新 , 默认值是 yes
Listener=no                                     ; yes/no , 开启监听能力 , 默认值是 no
Listener.Channels=0                             ; 整数列表 , 监听的频道 , 写好几个频道就同时监听这几个频道上的广播更新 , 取值范围 : 0 ~ 10000 , 默认值是 0
Listener.Owner=Self                             ; 作战方归属 , 监听这些作战方的广播更新
                                                ; 可用值 : All (无简写) , Self | S , Allies | A , Enemies | E , Neutral | N , 默认值是 Self (不区分大小写)
                                                ; 当需要匹配多种作战方时 , 多个值之间使用 "," 符号连接即可 , 栗如同时匹配己方和敌方 : Self,Enemies 或 S,E (简写可以混用 , 不要有空格)
Listener.Range=0                                ; 浮点数 , 需要广播源在这个范围才可以触发监听 , 负数或 0 = 不限制 , 默认值是 0 , 单位 : 格子 (开启会比较费性能)
Listener.ActiveMode=yes                         ; yes/no , yes = 主动监听模式 , no = 被动监听模式 , 默认值是 yes (通常来说【被动监听模式】会更费性能 , 请按需选择)

; 数值显示相关属性
; 生效状态的 Buff 才会显示数值
Digital=no                                      ; yes/no , 是否启用数值显示 , 默认值是 no
Digital.Primary=                                ; 数值显示设置 , 显示第 1 类数值 (显示哪些数值请见具体的 Buff 效果) , 不设置不显示 , 默认值是 空
Digital.Secondary=                              ; 数值显示设置 , 显示第 2 类数值 (显示哪些数值请见具体的 Buff 效果) , 不设置不显示 , 默认值是 空
Digital.Tertiary=                               ; 数值显示设置 , 显示第 3 类数值 (显示哪些数值请见具体的 Buff 效果) , 不设置不显示 , 默认值是 空
Digital.Quaternary=                             ; 数值显示设置 , 显示第 4 类数值 (显示哪些数值请见具体的 Buff 效果) , 不设置不显示 , 默认值是 空
Digital.Button=                                 ; 按钮显示设置 , 显示按钮 , 这是一个进行了特殊优化的【数值显示设置】 , 大部分【血条】相关的属性都彼此通用 , 不设置不显示 , 默认值是 空
Digital.Button.KeyBinds=                        ; 按钮绑定列表 , 有几个按钮写几个绑定 , 不写的算不绑定 , 详细情况请见【空白快捷键】部分
                                                ; 可用值 : 0 = 无绑定 , 1 - 6 = 第 1 到第 6 个快捷键 , 默认值是 0
                                                ; Show.Bars=no 时 , 按钮会隐藏从而导致无法点击 , 但是快捷键仍然有效
Digital.AutoOffset=yes                          ; yes/no , 是否会根据 Buff 的 Order 属性自动偏移 , 从而避免显示的数值和其他 Buff 重叠在一起 , 偏移的高度由【数值显示设置】的 Size 属性决定 , 默认值是 yes
Digital.BufferSpeed=0.005                       ; 浮点数 , 【血条】的缓冲残影的消失速度 (对【按钮】无效) , 速度是每帧减少的百分比 , 0.005 意味着每帧减少 0.5% , 200 帧会掉空 , 默认值是 0.005 , 单位 : 比例/帧
```

另见 [类型-Buff 参数设置](/Buff/子类型-Buff参数设置.md#子类型-Buff-参数设置)、[类型-判断单位条件](/Buff/子类型-判断单位条件.md#子类型-判断单位条件)、[类型-坐标运动轨迹](/Buff/子类型-坐标运动轨迹.md#子类型-坐标运动轨迹)、[类型-数值显示设置](/数值显示/类型-数值显示设置.md)、[类型-按钮显示设置](/数值显示/类型-按钮显示设置.md) 和 [空白快捷键](/功能扩展-空白快捷键.md)。

效果强度值说明另见 [概念：效果强度值叠加](/Buff/总体说明.md#概念效果强度值叠加) 和 [概念：效果强度值转换](/Buff/总体说明.md#概念效果强度值转换)。  
广播与监听说明另见 [概念：广播与监听](/Buff/总体说明.md#概念广播与监听)。