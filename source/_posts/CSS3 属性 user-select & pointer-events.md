---
title: CSS3 属性 user-select & pointer-events
author: GleanWu
tags:
  - CSS3
  - HTML
---

接下来介绍两个常用的 css 关于用户界面方面的 CSS3 属性, 今天在处理水印的时候用到了这两个，简单记录一下

### user-select

> 设置或检索是否允许用户选中文本

#### 相关介绍

- 默认值: text

#### 取值：

##### none

文本不能被选择

##### text

可以选择文本

##### all

当所有内容作为一个整体时可以被选择。如果双击或者在上下文上点击子元素，那么被选择的部分将是以该子元素向上回溯的最高祖先元素。

##### element

可以选择文本，但选择范围受元素边界的约束

[详情参考](https://www.html.cn/book/css/properties/user-interface/user-select.htm#a3)

### pointer-events

> 设置或检索在何时成为属性事件的 target

语法：
pointer-events：auto| none | visiblepainted | visiblefill | visiblestroke | visible | painted | fill | stroke | all

默认值：auto

适用于：所有元素

继承性：有

动画性：否

计算值：指定值

取值：
auto：
与 pointer-events 属性未指定时的表现效果相同。在 svg 内容上与 visiblepainted 值相同
none：
元素永远不会成为鼠标事件的 target。但是，当其后代元素的 pointer-events 属性指定其他值时，鼠标事件可以指向后代元素，在这种情况下，鼠标事件将在捕获或冒泡阶触发父元素的事件侦听器。
其他值只能应用在 SVG 上

说明：
设置或检索在何时成为属性事件的 target。
使用 pointer-events 来阻止元素成为鼠标事件目标不一定意味着元素上的事件侦听器永不会触发。如果元素后代明确指定了 pointer-events 属性并允许其成为鼠标事件的目标，那么指向该元素的任何事件在事件传播过程中都将通过父元素，并以适当的方式触发其上的事件侦听器。当然位于屏幕上在父元素上但不在后代元素上的鼠标活动都不会被父元素和后代元素捕获（将会穿过父元素而指向位于其下面的元素）。
对应的脚本特性为 pointerEvents。
