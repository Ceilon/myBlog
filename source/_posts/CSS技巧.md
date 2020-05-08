---
title: CSS技巧
date: 2019-08-01 16:31:28
categories: 代码
thumbnail: http://pvjq3azwj.bkt.clouddn.com/blog/20190801/pVWzkjLSwss9.png
---
### 1.万能居中的两种方式

> ##### 结构：

```html
<div id="dad">
	<div id="son">
	</div>
</div>
```

##### 方法一、

```css
#son｛
    position:absalute;
    top:0;
    left:0;
    bottom:0;
    right:0;
    margin:auto;
｝
```

##### 方法二、

```html
<head>
<style>
*{
    margin:0;
    padding:0:
}
a{
    display:inline-block:
    text-decoration:none;
    color:white;
    background:gray:
    text-aline:center;


    /*居中样式*/
    /*使元素的右上角的点在包含块中居中*/
    position:absolute;
    left:50%;
    top:50%;
    /*根据元素自身的宽高，进行移动，从而达到元素居中*/
    transform:translate(-50%,-50%);
}
</style>
</head>
<body>
<a href="javascript:;">按钮</a>
</body>
```

### 2.伪类选择器

##### 说明：控制区间内的标签样式

```less
//父级  
.secondBox{
    //子级所有:not(:last-child)表明除了最后一个的所有元素
      .secondBoxRow:not(:last-child){
        border-bottom: rgba(233, 233, 233, 0.8) solid 1px;
      }
    }

.secondBox{
    //子级所有:not(:last-child)表明除了最后一个的所有元素
      .secondBoxRow:nth-of-last-type(n+2){
        border-bottom: rgba(233, 233, 233, 0.8) solid 1px;
      }
    }
```

### 3.搜索引擎优化标签

> ##### 三大标签优化

```html
//搜索引擎了解网页的入口，和对网页主题贵树的最佳判断点
//1.标准长度baidu：28个中文，google，35个中文
//2.最先出现权重越高
//3.主关键词3次，副关键词一次
<title>京东(JD.COM)-正品低价、品质保障、配送及时、轻松购物！</title>
//网站介绍，最多120汉字
<meta name="description" content="京东JD.COM-专业的综合网上购物商城,销售家电、数码通讯、电脑、家居百货、服装服饰、母婴、图书、食品等数万个品牌优质商品.便捷、诚信的服务，为您提供愉悦的网上购物体验!"/>
//关键词搜索，6-8个关键词
    <meta name="Keywords" content="网上购物,网上商城,手机,笔记本,电脑,MP3,CD,VCD,DV,相机,数码,配件,手表,存储卡,京东"/>
```

### 4.运用超出隐藏让webpack解析成功

```css
  //height: 300px;
    text-indent: 2em;
    color: #afaeb1;
    overflow: hidden;
    text-overflow: ellipsis;
    display: -webkit-box;
    overflow: hidden;
    /*! autoprefixer: ignore next */
    -webkit-box-orient: vertical;
```

### 5.计算高度，用总高度减去预留高度

```css
height:calc(100vh-20px)
//less写法
height:calc(~"100vh-20px")
```

### 6.左右固定宽度，中间宽度自适应

* position定位

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
    <style>
      .container {
        height: 100%;
        /* position: absolute; */
      }
      .left,
      .right {
        position: absolute;
        top: 0;
        width: 200px;
        height: 100%;
        background-color: red;
      }
      .left {
        left: 0;
      }
      .right {
        right: 0;
      }
      .main {
        position: absolute;
        background-color: silver;
        height: 100%;
        left: 200px;
        right: 200px;
      }
    </style>
  </head>
  <body>
    <div class="container">
      <div class="left"></div>
      <div class="main"></div>
      <div class="right"></div>
    </div>
  </body>
</html>
```





