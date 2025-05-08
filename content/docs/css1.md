---
title: CSS BASIC1
categories:
  - web
abbrlink: 2701
date: 2024.7.11
tags: 

   - css 

---



# CSS

网页分为三个部分：

结构：html

表现：css

行为：js

# 书写样式

### 内联样式

```
<!DOCTYPE html>
<html lang="en`">
    <head>
        <meta charset="utf-8">
        <meta name="test" content="width-device-width">
        <meta http-equiv="x-ua-compatible" content="ie-edge">
        <time datetime="gogog"></time>
    </head>
    <body>
        <p style="color: red; font-size: 60px;">
            狗狗狗狗欧冠
        </p>
    </body>
</html>
```

简单的css及其展示

<!DOCTYPE html>
<html lang="en`">
    <head>
        <meta charset="utf-8">
        <meta name="test" content="width-device-width">
        <meta http-equiv="x-ua-compatible" content="ie-edge">
        <time datetime="gogog"></time>
    </head>
    <body>
        <p style="color: red; font-size: 60px;">
            我是📕
        </p>
    </body>
</html>

不推荐使用，样式只能对一个标签进行修改，不方便修改，开发时不推荐使用 

### 内部样式

将样式写道head的style标签里

```
<!DOCTYPE html>
<html lang="en`">
    <head>
        <meta charset="utf-8">
        <meta name="test" content="width-device-width">
        <meta http-equiv="x-ua-compatible" content="ie-edge">
        <time datetime="gogog"></time>
        <style>
            p{
                color: red; font-size: 60px
            }
        </style>
    </head>
    <body>
        <p>狗狗狗狗欧冠</p>
    </body>
</html>
```

内部方式，不能跨页面使用。

### 外部css文件中（开发最佳的）

css写在外部标签中，再使用link标签插入

```
<!DOCTYPE html>
<html lang="en`">
    <head>
        <meta charset="utf-8">
        <meta name="test" content="width-device-width">
        <meta http-equiv="x-ua-compatible" content="ie-edge">
        <time datetime="gogog"></time>
        <link rel="stylesheet" href="./style.css"

    </head>
    <body>
        <p>狗狗狗狗欧冠</p>
    </body>
</html>
```

只要是想使用这个css都可以使用

可以使用浏览器的缓存机制，从而加载网页的加载速度，提高用户的体验

## css基本语法

```
/*
aaaaaa
*/
```

css的注释

css选择器+css声明块

选择器：通过选择器可以选定页面中的指定元素

声明块：是用来为元素来设置样式的，由一个个的声明组成

```
p{
    width: 50px;
    color: aqua;
    
}
```

一个样式名和样式值组成



# 常用选择器

### 元素选择器

标签名{

},选中了全部，不太方便

```
p{

}
```

### id选择器

```
#id{

}


```



### class选择器

class属性和id类似，不用的是class可以重复使用，通过class属性用来对元素进行分组

```
.bule{

}
```

一个元素可以用多个类，多个class用空格隔开

### 通配选择器

```
*{

}
```

## 复合选择器

```
div.class{
                
            }
```

语法：选择器1.选择器2.选择器.....{

}

**如果有元素选择器必须要元素选择器开头**

```
h1,span{

}
```

同时选择多个选择器

## 关系选择器

父子兄弟选择器：



父子关系：直接包含

祖先后代关系：直接或间接包含

兄弟关系

父子：

```
div.box > p{

}
```

后代：

```
div p{

}
```

兄弟：

```
p + span{
下边一个
}
```

```
p ~ span{
下面所有的
}
```

选的都是下边的兄弟

## 属性选择器

title标签的标题

```
p[title]{

}
```

选择包含title的属性的元素

```
p[title=aaaa]{

}
```

选择特定属性的元素

```
p[title^=aaaa]{

}
```

选择以...开头的元素

```
p[title$=aaaa]{

}
```

以...结尾

```
p[title*=aaaa]{

}
```

含有就行

## 伪类选择器(不存在的类)

一个冒号

伪类（第一个元素，被点击的元素，鼠标移入地元素）

```
ul > li:first-child{
                
            }
```

第一个子元素

```
ul > li:last-child{
                
            }
```

最后一个

```
 ul > li:nth-child(){       
 }
```

第n个范围到正无穷

```
 ul > li:2nth-child(2n){       
 }
```

表示选择偶数位的元素

```
 ul > li:nth-child(2n+1){       
 }
```

表示基数

:first-of-type

...同类型的一类的进行排序来选择

```
 ul > :li:not(:first-child(3))
```

## 超链接的伪类

```
<!DOCTYPE html>
<html lang="en`">
    <head>
        <meta charset="utf-8">
        <meta name="test" content="width-device-width">
        <meta http-equiv="x-ua-compatible" content="ie-edge">
        <time datetime="gogog"></time>
        <style>
            a.link{
                color: rebeccapurple
            }


        </style>
    </head>
    <body>
        <a href="https://mengnankk.asia"></a>

       
    </body>
</html>
```

link表示的是正常的链接

```
<!DOCTYPE html>
<html lang="en`">
    <head>
        <meta charset="utf-8">
        <meta name="test" content="width-device-width">
        <meta http-equiv="x-ua-compatible" content="ie-edge">
        <time datetime="gogog"></time>
        <style>
            a.visited{
                color: rebeccapurple
            }


        </style>
    </head>
    <body>
        <a href="https://mengnankk.asia"></a>

       
    </body>
</html>
```

visited是访问过的链接

这个只能改变访问过链接的颜色

- hover表示鼠标移入的状态
- active表示鼠标点击的状态

## 伪元素选择器

P::first-letter

```
P::first-letter{
                
            }
```

第一个

```
first-line第一行
```

```
selection选中的
```

```
::before

::after
```

开始和结束

必须集合content来使用，通过css添加

## 样式的继承

为一个元素设置的样式，也会应用到它的后代元素中

在继承上通用的样式，应用到共同的祖先元素上

并不是所有的样式都会继承，例如背景相关的，布局相关的不会被继承

## 选择器的权重

样式的冲突：通过不同的选择器选择相同的元素，为相同的元素设置不同的值时，就会出现冲突。就会由选择器的权重决定

权重：

- 内联样式	1000
- id选择器      100
- 类的伪类      10
- 元素选择器    1

**比较时，需要把所有选择器的优先级相加，最后优先级越高，越优先显示（分组选择器的单独计算的）**

**选择器的累加，不会超过其最大的数量级，无法跨越数量级**

**如何优先级相等，则优先使用靠下的样式**

**统配选择器优先级时0，继承的样式没有优先级**

!important在某一个样式后加上，就会变成最高级的优先级，开发中一定要慎用

## 单位

### 长度的单位

像素：显示器是由一个个的小点构成的，像素越小的屏幕，显示的效果越清晰

所以同样的200像素在不同的设备下显示的效果不同

px是像素点

也可以设置百分比，设置为父元素的属性的百分比

百分比的好处，可以是子元素跟随父元素的改变而改变

### em/rem

em是相对于元素的字体大小计算的

1em = 1font-size

em的单位会根据字体的大小改变而改变

rem是根据根元素的字体大小的改变而改变

移动端使用rem比较多一点

## 颜色

在css中使用颜色名来设置各种颜色，但是不太方便

颜色的编号来使用

rgb值来表示颜色，通过三种颜色的浓度来调配出不同的颜色

就是红色绿色蓝色几种颜色的占比

每一种颜色的范围在0~255之间（0~100%）之间

```
.box1{
	color: rgb(255,255,10);
}

```

通过这个来

三个值都是0为黑色，都是255就是白色

十六进制的rgb值

#ff000;表示红色

两位两位重复，可以简写

#ffffff    =  #fff

hsl hsla来表示颜色

- h 色相
- s 饱和度
- l亮度

hsl(0~360,0~100，0~100)亮度0是黑色
