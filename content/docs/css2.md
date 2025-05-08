---
title: CSS BASIC2
categories:
  - web
abbrlink: 2701
date: 2024.7.25
tags: 

   - css 

---

# CSS  BASIC2

## _layout布局

### 文档流

网页是一个多层的结构，一层加一层，层层叠叠，通过css来设置每一层的样式，作为用户只能看到最顶上的一层

最底下的一层成为文档流，我们所创建的元素默认都是在文档流里进行排列，元素主要有两个状态，在文档流中和脱离文档流两个状态

文档流的特点：

- 块元素在页面中独占一行，默认宽度是父元素的全部，会把父元素盛满，默认高度是被内容撑开，自上而下垂直排列
- 行内元素不会独占一行，只占自身的大小，自左向右水平排列。如何一行中不足以容纳，会换到第二行继续自左向右排列，行内元素的默认宽度和高度都是被内容撑开

### 盒子模型

首先确定元素的形状和大小，css将所有元素都设置成了一个矩形的盒子

每一个盒子都由一下部分组成：

- 内容区（content)
- 边块（border)
- 内边距（padding)
- 外边距（margin)

内容区：元素中的所有子元素和内容文本都在内容区中排列

width和height设置高度和宽度

设置边块至少下面三个元素



```
	.box1{border-width: 10px;

​        border-color: red;

​        border-style: solid;

​      }


```

边块的大小会影响盒子的大小

#### 边框



```
	.box1{border-width: 10px;

​        border-color: red;

​        border-style: solid;

​      }


```

border-width会有默认值，上下左右都是10个像素。写4个值的化，先上右下左的顺序来排列

写3个值，上 左右 下。写两个值是上下 左右 

除了这个还右一组

```
border-top: ;
```

这些来设定边框

border-color用来指定颜色，规则和上面一样

如何省略，则自动使用color值

border-style:

- solid	实线
- dotted 点状虚线
- dashed 虚线
- double 双线

四个边框和上面的规则一样

默认值为：none为没有

#### 内边距

一共有四个方向的padding 分别是top/bottom/right/left

内边距的设置会影响到盒子的大小，背景颜色延申到内边距上

**盒子的大小是由内容区，内边距和边块的大小决定的**这些是可见框，外边距是不可见的

padding：这个属性的规则和前面的属性是一致的

#### 外边距

margin不会影响盒子的大小，但是会影响盒子的位置。一共有四个方向的外边距

元素默认情况下是自左向右的，往左往上会移动元素自己，右和下会移动其他元素

**除了正值，还可以设置负值。**

margin：这个属性的规则和前面的属性设定是一样的

会影响盒子实际的大小，可见框是不会影响的

#### 盒子的水平布局

水平方向的位置由以下几个属性共同决定

- margin-left
- border-left
- padding-left
- width
- padding-right
- border-right
- margin-right

一个元素在父元素中水平的布局必须满足：

**上面的七个值的和，等与其父元素内容区的宽度**

以上等式必须满足，如果等式不成立，则成为过度约束，等式会自动调整

如果这七个值没有auto的值，那么会调整margin-right的值

有三个值可以设定为auto

- width
- margin-left
- margin-right

更改的时候会调整auto的值来使等式成立

如果将width和一个margin设置为auto那么宽度会调整为最大

三个值都为auto那么宽度最大

**如果将两个外边距设置为auto那么两个外边距平分，宽度不变。使用这个特点来是元素居中**

#### margin塌陷问题

给子元素的margin的值被父元素抢走

- 给父元素加上一个padding
- 给父元素加上一个border
- overflow：hiddenl;专门控制溢出的元素的控制

#### margin合并的问题

margin的合并只是存在在上下兄弟之间

使用float属性	float:left使用这个会混乱其他的元素，所以就直接写就行了，不需要考虑其他的了。

### 处理内容的溢出

在容器上加属性，使用overflow属性

hidden隐藏 scrool滚动条 auto自动条 默认属性visable显示

还可以使用overflow-x和overflow-y两个属性只能一样的效果比较好

### 隐藏元素的两种方式

- display:none;这样就可以隐藏了这个是不占位置的
- visibility：hidden;这个属性的隐藏是占据位置的

### 样式的继承

**font-weight：bold字体加粗**

**font-family: 字体 修改字体的属性**

在f12开发者工具中国，亮的部分是能够继承的，不亮的部分是不能继承的

字体属性，文本属性（除了vertical-align）文字属性等是可以继承的

和盒子模型等和布局无关的属性是可以继承的，盒子属性等是不能继承的

### 元素的默认样式

- 超链接：继承过来的优先级比较低，不如默认属性
- h1标题也是用默认样式的
- 段落p
- ul列表
- body

