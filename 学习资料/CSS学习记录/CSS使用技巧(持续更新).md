## 常用垂直居中方式

``` 
html,body{
height:100%;width:100%;
margin:0;padding:0
}                             //清除默认样式
```

1. 通过position:relative   向下偏移50%  {top:50%},距离顶部自身一半距离: { margin-top:height/2}

2. 使用flex布局:  父级元素使用:

   ``` 
   display:flex;
   align-items: center; /*定义body的元素垂直居中*/
   justify-content: center; /*定义body的里的元素水平居中*/
   ```

 ## 水平居中方式

``` 
text-align:center;
margin:0 auto;
postition:absolute;left:50%;
```

样例:使用行内块元素加text-aligin实现  '分页':

分页<a href="https://www.w3cplus.com/css/elements-horizontally-center-with-css.html" alt="分页">

## 浮动及清除

浮动可以左右移动,直到遇到一个浮动框或者遇到它外边缘的包含框:它不影响块级元素的布局,只影响内联元素的布局

浮动会出现的问题:高度塌陷. 如果浮动的元素高于父级的元素,会使子元素的高度超出父元素的宽度,父元素的高度不会随子元素的高度而撑开:    

<a href="http://www.divcss5.com/jiqiao/j406.shtml">

直接影响:

1. 背景无法显示,毕竟高度已经矮了
2. 边框不能撑开
3. margin padding 设置不能正确显示

此时<b>清除浮动</b>可以使父级元素跟子级元素高度一致:

``` 
1.在浮动元素下面添加一个空标签,标签中设置:
clear:both
2.在父级元素定义
overflow:hidden
此时无法使用position,超出的尺寸会被隐藏
3.使用伪元素after清除浮动
.parent:after{
content:"."; display:block; height:0; visibility:hidden; clear:both;
}
```

## 三栏布局

### 两边固定中间自适应

1. ``` 
   两边浮动,中间margin设置左右距离.中间必须放在左右后面
   ```

2. ``` 
   三块都使用绝对定位,左右设置宽度和左右0,中间设置离两边距离,
   ```

3. ``` 
   flex布局:左右设置距离,中间:flex:1.父元素设置display
   ```

4. ``` 
   grid布局:网格布局
   ```

5. ``` 
   双飞翼布局:三个全部设置浮动,右边部分margin-left:-100%,左边部分margin-left:-300%
   圣杯布局:
   ```

## 使用flex实现fixed效果

``` 
https://www.cnblogs.com/moumou0213/p/6500956.html
```

## BFC(block formart context 块级上下文)

BFC格式化上下文,可以把BFC当做页面中的一个迷你布局,一旦创建一个BFC,它其中的所有元素都会被它包裹.也就是清除浮动.

**BFC一套渲染规则**，它决定了其子元素将如何定位，以及和其他元素的关系和相互作用

产生BFC的条件满足一条即可:

1. overflow不为visible

2. float不为none

3. position为absolute或者fixed

4. display:inline-block,flex等

5. 根元素默认是BGC

   BGC特性

   1. 内部的Box会在垂直方向，一个接一个地放置。
   2. Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠
   3. 每个元素的margin box的左边， 与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。
   4. BFC的区域不会与float box重叠。
   5. BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。
   6. 计算BFC的高度时，浮动元素也参与计算

## BFC可以解决的问题

1. a是浮动元素,b是正常文档流的元素,被c包裹

   此时 b把a给包起来了,因为浮动会是周围元素围绕它,

   这时使用  overflow:hidden解决这个问题.

2. a和b都设置了margin都是100px,一般情况下a和bmargin重叠了

   此时给a添加一个div并设置overflow就能解决这个问题