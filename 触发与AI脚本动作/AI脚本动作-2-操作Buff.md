# AI脚本动作-操作 Buff

## 50100,n Buff 挂载

按照设置为当前作战小队的所有成员挂载 Buff。  
Buff 来源是作战小队的队长。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; Buff 列表 , 挂载这些 Buff
BuffSets=                                       ; Buff 参数设置列表 , 挂载 Buff 时会合并此设置 , 不填写则使用 Buff 自身的默认值
```



## 50101,n Buff 激发

按照设置激发当前作战小队的所有成员身上的 Buff。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; Buff 列表 , 激发这些 Buff
BuffSets=                                       ; Buff 参数设置列表 , 激发 Buff 时会合并此设置 , 不填写则无变化仅激发
```



## 50102,n Buff 结束

按照设置结束当前作战小队的所有成员身上的 Buff。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; Buff 列表 , 结束这些 Buff
```



## 50103,n Buff 移除

按照设置移除当前作战小队的所有成员身上的 Buff。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; Buff 列表 , 移除这些 Buff
```



## 50104,n Buff 修改

按照设置修改当前作战小队的所有成员身上的 Buff 的属性。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; Buff 列表 , 修改这些 Buff
BuffSets=                                       ; Buff 参数设置列表 , 修改 Buff 时会合并此设置 , 不填写则无效果
```



## 50105,n Buff 转换

按照设置转换当前作战小队的所有成员身上的 Buff。  
Buff 来源是作战小队的队长。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; Buff 列表 , 转换这些 Buff
ThoseBuffs=                                     ; Buff 列表 , 上面的 Buff 转换为这些 Buff , 一一对应
BuffSets=                                       ; Buff 参数设置列表 , 转换后的 Buff 会合并此设置 , 不填写则使用 Buff 自身的默认值
```

### 注意

* 在 Buff 参数设置中，`Duration=0` 会继承转换前的 Buff 的挂载持续时间，`Power=0` 会继承转换前的 Buff 的效果强度值。