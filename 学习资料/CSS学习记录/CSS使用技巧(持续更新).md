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