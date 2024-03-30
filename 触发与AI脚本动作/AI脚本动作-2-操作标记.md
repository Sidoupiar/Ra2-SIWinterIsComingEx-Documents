# AI脚本动作-操作标记

## 50100,n 标记挂载

按照设置为当前作战小队的所有成员挂载标记。  
标记来源是作战小队的队长。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseMarks=                                     ; 标记列表 , 挂载这些标记
MarkSets=                                       ; 标记参数设置列表 , 挂载标记时会合并此设置 , 不填写则使用标记自身的默认值
```



## 50101,n 标记激发

按照设置激发当前作战小队的所有成员身上的标记。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseMarks=                                     ; 标记列表 , 激发这些标记
MarkSets=                                       ; 标记参数设置列表 , 激发标记时会合并此设置 , 不填写则无变化仅激发
```



## 50102,n 标记结束

按照设置结束当前作战小队的所有成员身上的标记。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseMarks=                                     ; 标记列表 , 结束这些标记
```



## 50103,n 标记移除

按照设置移除当前作战小队的所有成员身上的标记。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseMarks=                                     ; 标记列表 , 移除这些标记
```



## 50104,n 标记修改

按照设置修改当前作战小队的所有成员身上的标记的属性。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseMarks=                                     ; 标记列表 , 修改这些标记
MarkSets=                                       ; 标记参数设置列表 , 修改标记时会合并此设置 , 不填写则无效果
```



## 50105,n 标记转换

按照设置转换当前作战小队的所有成员身上的标记。  
标记来源是作战小队的队长。

位于 `rulesmd.ini`：

```ini
[SomeDataPackType]
; 数据属性
TheseMarks=                                     ; 标记列表 , 转换这些标记
ThoseMarks=                                     ; 标记列表 , 上面的标记转换为这些标记 , 一一对应
MarkSets=                                       ; 标记参数设置列表 , 转换后的标记会合并此设置 , 不填写则使用标记自身的默认值
```

### 注意

* 在标记参数设置中，`Duration=0` 会继承转换前的标记的挂载持续时间，`Power=0` 会继承转换前的标记的效果强度值。