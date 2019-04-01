# Sass快速入门:嵌套导入注释保持条理和可读

### 变量声明

1. 使用$ 声明变量:建议把反复使用的css属性值定义成变量,然后通过变量名来引用它们,无需重复写
2. 在{...}外声明的变量到处可用,在{...}声明的变量只在本{...}中可使用

### 变量引用

``` 
$highlight-color: #F90;
$highlight-border: 1px solid $highlight-color;
.selected {
  border: $highlight-border;
}

//编译后

.selected {
  border: 1px solid #F90;
}
```

1. 可以在变量中使用变量
2. 尽量使用中划线,少使用下划线.在sass中两者互通

### 嵌套CSS规则

以后代选择器举例:

``` 
$name:blue;
#div-style {
	article {
        h1 {color:$name}
        p{color:$name}
	}
	aside {
        
	}
    
}                        
   //等价于
#div-style article h1 {}
#div-style article p {}
#div-style aside {}
```

### & 父级选择器的标识符

``` 
article a {
color:blue;
&:hover {color:red}
}
//如果不加& 会为article所有元素加上hover效果.
//此时&指代 article a   只为a加上hover效果.
另一种用法
#content aside {
    body.ie & {color:blue} 
}                        等价于    body.ie #content aside {color:blue}
```

### 并集选择器

``` 
#container {
    a,h1,h2 {
        color:blue
    }
}
等价于
#container a,#container h1,#container h2 {
    color:blue
}
```

### 各类其他选择器

``` 
子代选择器:  只选子代,孙代不选
#container > a {}
同层相邻选择器:   只选后面紧跟的第一个元素,这两元素有相同的父元素
#container + p {color:red}
同层全体组合器:    只选跟着的所有元素,这两个元素有相同的父元素

```

### 属性嵌套

``` 
nav {
  border: 1px solid #ccc {
  left: 0px;
  right: 0px;
  }
}
等价于
nav {
  border-left: 1px solid #ccc ;
  border-left: 0px;
  border-right: 0px;
  }
}
```

### 导入SASS文件

@important不同于css的,在生成css文件时就导入相关文件.css是执行到才导入会很慢

``` 
@import "colors"    colors.scss           可省略后缀名自动识别scss,sass
```

### 局部文件

``` 
局部文件可以被多个不同的文件引用。当一些样式需要在多个页面甚至多个项目中使用时，这非常有用
@import "themes/night-sky";     一般局部文件需要加上"_"  但是使用时可以省略,声明时不能省略!
        _night-sky.scss
```

### 默认变量值

如果需要更改别人提供的可复用的局部文件,使用  <b>默认变量值</b>

``` 
多次声明同一个变量,最后一个覆盖前面的变量

$fancybox-width: 400px !default;
.fancybox {
width: $fancybox-width;
}
```

如果这个变量被声明赋值了,那就用它声明的值,否则就用这个默认值

在上例中，如果用户在导入你的`sass`局部文件之前声明了一个`$fancybox-width`变量，那么你的局部文件中对`$fancybox-width`赋值`400px`的操作就无效。如果用户没有做这样的声明，则`$fancybox-width`将默认为`400px`



### 嵌套导入

假设有一个文件:   _blue-theme.scss

``` 
aside {
color:red;
}
```

现在有个文件要导入它:

```` 
.blue-theme {@import "blue-theme"}
生成的结果就是这样:
.blue-theme {
  aside{color:red;}
  }
结果显示它被包含在里面了
````

### 原生导入

以下几种情况下使用 "@import" 会使浏览器导入原生css

``` 
1.被导入的文件的名字以.css结尾
2.被导入的文件的名字是一个url地址
3.被导入文件的名字是css的url()值
```

所以尽量不要直接@import css文件,把后缀改成.scss即可直接导入

### 静默注释

一般来说注释谁都能看到,但是你并不希望每个浏览源码的人都能看到注释所以

使用了一种不同于css标准的注释格式 //  即静默注释

/**/会出现在css文件中.但是 //  不会

## 高级特性

### 混合器

处理大段的重用样式的代码:      定义 @mixin     使用 @include

``` 
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```

```
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
} 
```

这不就是跟定义变量差不多么

<b>何时使用它</b>:判断一组属性是否应该组合成一个混合器，一条经验法则就是你能否为这个混合器想出一个好的名字:展示效果的名字  

<b>混合器中也可以加入选择器还有各种东西</b>:

``` 
@mixin no-bullets {
  list-style: none;
  li {
    list-style-image: none;
    list-style-type: none;
    margin-left: 0px;
  }
}
```

``` 
使用时
ul.plain {
  color: #444;
  @include no-bullets;
}
等价于
ul.plain {
  color: #444;
  list-style: none;
}
ul.plain li {
  list-style-image: none;
  list-style-type: none;
  margin-left: 0px;
}
```

### 给混合器传参

``` 
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
```

使用 @include 时,你可以加上变量

``` 
a {
  @include link-colors(blue, red, green);
}
```

有时候会很难区分每个参数是什么意思.可以使用

``` 
name:value 形式来传值
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
```

有时不想去赋值了,可以自带初值也就是默认值,默认值可以是具体值,也可以是变量

``` 
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
//@include link-colors(red)   此时hover的初值是变量normal,visited的初值也是normal
```

## 选择器继承

``` 
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```

这不就跟混合器又差不多么...但是继承有个好处,就是被继承的相干项也会被继承下来

``` 
.error a{  //应用到.seriousError a
  color: red;
  font-weight: 100;
}
```

## 混合器和继承的区分

混合器主要用于样式的重用,继承主要用于语义化的重用,比如这个类是属于那个类的

继承一个html元素的样式

``` 
.disabled {
  color: gray;
  @extend a;
}
这样.disabled就继承了所有a相关联的样式
```

继承遵从`css`层叠的规则。当两个不同的`css`规则应用到同一个`html`元素上时，并且这两个不同的`css`规则对同一属性的修饰存在不同的值，`css`层叠规则会决定应用哪个样式。相当直观：通常权重更高的选择器胜出，如果权重相同，定义在后边的规则胜出

通常使用继承会让你的`css`美观、整洁。因为继承只会在生成`css`时复制选择器，而不会复制大段的`css`属性

# 不要用后代选择器去继承

# 虽说不知真假还是避免使用继承

https://www.w3cplus.com/preprocessor/mixins-better-for-performance.html