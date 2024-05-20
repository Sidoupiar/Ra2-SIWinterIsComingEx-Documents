# AI脚本动作-操作增益

## 50100,n 增益挂载

按照设置为当前作战小队的所有成员挂载增益。  
增益来源是作战小队的队长。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; 增益列表 , 挂载这些增益
BuffSets=                                       ; 增益参数设置列表 , 挂载增益时会合并此设置 , 不填写则使用增益自身的默认值
```



## 50101,n 增益激发

按照设置激发当前作战小队的所有成员身上的增益。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; 增益列表 , 激发这些增益
BuffSets=                                       ; 增益参数设置列表 , 激发增益时会合并此设置 , 不填写则无变化仅激发
```



## 50102,n 增益结束

按照设置结束当前作战小队的所有成员身上的增益。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; 增益列表 , 结束这些增益
```



## 50103,n 增益移除

按照设置移除当前作战小队的所有成员身上的增益。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; 增益列表 , 移除这些增益
```



## 50104,n 增益修改

按照设置修改当前作战小队的所有成员身上的增益的属性。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; 增益列表 , 修改这些增益
BuffSets=                                       ; 增益参数设置列表 , 修改增益时会合并此设置 , 不填写则无效果
```



## 50105,n 增益转换

按照设置转换当前作战小队的所有成员身上的增益。  
增益来源是作战小队的队长。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseBuffs=                                     ; 增益列表 , 转换这些增益
ThoseBuffs=                                     ; 增益列表 , 上面的增益转换为这些增益 , 一一对应
BuffSets=                                       ; 增益参数设置列表 , 转换后的增益会合并此设置 , 不填写则使用增益自身的默认值
```

### 注意

* 在增益参数设置中，`Duration=0` 会继承转换前的增益的挂载持续时间，`Power=0` 会继承转换前的增益的效果强度值。