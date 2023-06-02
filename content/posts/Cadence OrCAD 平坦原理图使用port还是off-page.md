---
title: "Cadence OrCAD 平坦原理图使用port还是off-page？"
date: 2023-06-02
draft: False
math: katex
summary: 
tags: ["OrCAD"]
---



使用`OrCAD 16.6`。（17.2简单测试好像也有这个问题，不知道更新的版本是否不同）

# 结论
平坦原理图中，`个人推荐使用off-page connector`. 

# 使用Port的问题
主要是DRC的问题。port名称不匹配造成单端网络时，在平坦原理图中`DRC不会报错`。

比如，下图两页中的`data1`信号，一个接了`data1 port`，另一个接了 `data3 port`，但是DRC不会报错。感觉这容易导致设计错误。

![](/images/img_2023-06-02.png)

# 删除port的情况
删除port，如果两个net alias名称相同，DRC会有警告。

![](/images/img_2023-06-02-1.png)

```
Checking For Single Node Nets
WARNING(ORCAP-1600): Net has fewer than two connections data1 
                    SCHEMATIC1, PAGE1  (3.50, 1.60) 
WARNING(ORCAP-1600): Net has fewer than two connections data1 
                    SCHEMATIC1, PAGE2  (3.80, 2.40) 

Checking For Unconnected Bus Nets
WARNING(ORCAP-1611): Two nets in same schematic have the same name, but there is no off-page connector data1 
                    SCHEMATIC1, PAGE2  (3.80, 2.40) 
```


# 使用off-page的情况
使用off-page connector，但是名称不匹配时，DRC也会有警告。

![](/images/img_2023-06-02-2.png)

```
Checking For Single Node Nets
WARNING(ORCAP-1600): Net has fewer than two connections data1 
                    SCHEMATIC1, PAGE1  (3.50, 1.60) 
WARNING(ORCAP-1600): Net has fewer than two connections data3 
                    SCHEMATIC1, PAGE2  (3.80, 2.40) 

Checking For Unconnected Bus Nets

Checking Off-Page Connections
WARNING(ORCAP-1613): No matching off-page connector data1 
                    SCHEMATIC1, PAGE1  (3.90, 1.60) 
WARNING(ORCAP-1613): No matching off-page connector data3 
                    SCHEMATIC1, PAGE2  (4.20, 2.40) 
```


# 总结

加了port以后，DRC中
- 悬空网络的警告被抑制
- port不会像off-page connector一样检查名称匹配

总之就是port名称不匹配的错误就很难被发现。（似乎没找到相关的DRC设置）