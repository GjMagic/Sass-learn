#sass语法学习
---
####一、编译
######1.手动编译sass
>sass sass/style.scss:css/style.css

######2.自动编译sass
>sass --watch sass:css

####二、修改编译输出的css格式
######0.修改格式的代码:
>sass --watch sass:css --style expanded

######1.默认嵌套格式(nested)
```
  ul {
    font-size: 12px; }
    ul li {
      list-style: none; }
```

######2.紧凑格式(compact)
```
ul { font-size: 12px; }
ul li { list-style: none; }
```

######3.扩展格式(expanded)开发常用格式
```
ul {
  font-size: 12px;
}
ul li {
  list-style: none;
}
```

######4.压缩格式(compressed)生产环境使用
```
ul{font-size:12px}ul li{list-style:none}
```

####三、语法
######1.定义变量：
```
$primary-color: #1265b6;
$primary-border: 1px solid $primary-color;

div.box {
  background-color: $primary-color;
}

h1.page-header {
  border: $primary-border;
}
```
######2.嵌套：
(1) 语法嵌套：
>使用&的语法会引用父选择器

```
sass语法:
.nav {
  height: 100px;
  ul {
    margin: 0;
    li {
      list-style: none;
      float: left;
      width: 100px;
      height: 100px;
      &:hover {
        color: #ffffff;
      }
    }
  }
  & &-text {
    font-size: 15px;
  }
}


css语法:
.nav {
  height: 100px;
}
.nav ul {
  margin: 0;
}
.nav ul li {
  list-style: none;
  float: left;
  width: 100px;
  height: 100px;
}
.nav ul li:hover {
  color: #ffffff;
}
.nav .nav-text {
  font-size: 15px;
}
```

(2) 属性嵌套：
>属性嵌套时，写成对象的形式
```
sass语法：
body {
  font: {
    family: Helvetica;
    size: 15px;
    weight: normal;
  }
}

.nav {
  border: 1px solid $primary-color {
    left: 0;
    right: 0;
  }
}

css语法：
body {
  font-family: Helvetica;
  font-size: 15px;
  font-weight: normal;
}

.nav {
  border: 1px solid #1265b6;
  border-left: 0;
  border-right: 0;
}
```
######3.mixin混合
(1)语法：
```
@mixin 名字 [(参数1, 参数2)] {
  ...
}
```
(2)例子：
>@mixin用来定义，就像js里的function一样。@include用来调用。
@mixin里面也可以嵌套等
```
sass语法：
@mixin alert($text-color, $background-color) {
  color: $text-color;
  background-color: $background-color;
  a {
    color: darken($text-color, 10%);
  }
}

.alert-warning {
  @include alert(#1234b4, #456cd4);
}

.alert-info {
  @include alert($background-color: #423de2, $text-color: #ffffff)
}


css语法：
.alert-warning {
  color: #1234b4;
  background-color: #456cd4;
}
.alert-warning a {
  color: #0d2786;
}

.alert-info {
  color: #ffffff;
  background-color: #423de2;
}
.alert-info a {
  color: #e6e6e6;
}
```

######4.@extend继承
>使用关键字@extend。继承会继承所有
```
sass语法：
.console {
  margin-left: 5px;
}

.console a {
  font-size: 12px;
}

.console-log {
  @extend .console;
  background: #ffffff;
}


css语法：
.console, .console-log {
  margin-left: 5px;
}

.console a, .console-log a {
  font-size: 12px;
}

.console-log {
  background: #ffffff;
}
```

######5.Partials 与 @import
>在一个scss文件里包含其他scss文件，减少HTTP请求

(1)语法：
```
sass语法：
@import "base"(不需要加_和.scss)


css语法：
body {
  padding: 0;
  margin: 0;
}
```

######6.注释
(1)sass中文注释编译出错解决办法：
  
方法1：(试验后法一无用，参考方法2)
找到ruby的安装目录，里面也有sass模块，如这个路径：

C:\Ruby\lib\ruby\gems\1.9.1\gems\sass-3.3.14\lib\sass

2.0

在这个文件里面engine.rb，添加一行代码（同方法1）

Encoding.default_external = Encoding.find('utf-8')
放在所有的require XXXX 之后即可。


方法2：
在sass文件开头写上:
@charset "utf-8";

(2)注释使用：
```
/* 多行注释会包含在没有压缩之后的CSS里面 */

// 单行注释不会出现在CSS里面

/* 
  1、CSS输出格式为压缩格式(compressed)时，多行、单行注释都不存在 
  2、如果一定要在压缩格式里面显示注释，则加！即可
    /*！ 强制注释 */
*/
```