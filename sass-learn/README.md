# sass语法学习
---
#### 一、编译
###### 1.手动编译sass
>sass sass/style.scss:css/style.css

###### 2.自动编译sass
>sass --watch sass:css

#### 二、修改编译输出的css格式
###### 0.修改格式的代码:
>sass --watch sass:css --style expanded

###### 1.默认嵌套格式(nested)
```javascript
  ul {
    font-size: 12px; }
    ul li {
      list-style: none; }
```

###### 2.紧凑格式(compact)
```javascript
ul { font-size: 12px; }
ul li { list-style: none; }
```

###### 3.扩展格式(expanded)开发常用格式
```javascript
ul {
  font-size: 12px;
}
ul li {
  list-style: none;
}
```

###### 4.压缩格式(compressed)生产环境使用
```javascript
ul{font-size:12px}ul li{list-style:none}
```

#### 三、语法
###### 1.定义变量：
```javascript
$primary-color: #1265b6;
$primary-border: 1px solid $primary-color;

div.box {
  background-color: $primary-color;
}

h1.page-header {
  border: $primary-border;
}
```
###### 2.嵌套：
(1) 语法嵌套：
>使用&的语法会引用父选择器

```javascript
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
```javascript
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
###### 3.mixin混合
(1)语法：
```javascript
@mixin 名字 [(参数1, 参数2)] {
  ...
}
```
(2)例子：
>@mixin用来定义，就像javascript里的function一样。@include用来调用。
@mixin里面也可以嵌套等
```javascript
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

###### 4.@extend继承
>使用关键字@extend。继承会继承所有
```javascript
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

###### 5.Partials 与 @import
>在一个scss文件里包含其他scss文件，减少HTTP请求

(1)语法：
```javascript
sass语法：
@import "base"(不需要加_和.scss)


css语法：
body {
  padding: 0;
  margin: 0;
}
```

###### 6.注释
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
```javascript
/* 多行注释会包含在没有压缩之后的CSS里面 */

// 单行注释不会出现在CSS里面

/* 
  1、CSS输出格式为压缩格式(compressed)时，多行、单行注释都不存在 
  2、如果一定要在压缩格式里面显示注释，则加！即可
    /*！ 强制注释 */

```

#### 四、函数
###### 1.数据类型
>1、在命令行输入 sass -i即可，打开sass交互功能
>2、使用type-of()来判断数据类型
```
  (1)>> type-of(1)
    "number"
  (2)>> type-of(red)
    "color"
  (3)>> type-of(hello)
    "string"
  (4)>> type-of(1px solid #fff)
    "list"
```

###### 2.数字计算
>1、在命令行输入 sass -i即可，打开sass交互功能
>2、符合一切加减乘的规则，但进行除法运算时需要加括号，因为 / 在css中有别的用法
```
  (1)>> 3px + 2px
    5px
  (2)>> 3px * 2
    6px
  (3)>> 3px * 2px
    6px*px
  (4)>> (6px / 2)
    3px
  (5)>> (6px / 2px)
    3
  (6)>> 6px / 2
    6px/2
```

###### 3.数字函数
>1、abs()取绝对值、round()四舍五入、ceil()向上取整、floor()向下取整
>percentage()取百分数、min()取最小值、max()取最大值

###### 4.字符串
>字符串只有相加的功能，没有减、乘、除。

###### 5.字符串函数
>to-upper-case(), to-lower-case(), str-length(), str-index(), str-insert()
```
$emao: "hello emao"
>>to-upper-case($emao)使字符串大写
"HELLO EMAO"
>>to-lower-case($emao)使字符串小写
"hello emao"
>>str-length($emao)求变量字符串长度
10
>>str-index($emao, "hello")求指定字符串在变量的位置下标
1
>>str-insert($emao, ".net", 11)向变量的某个位置插入某个字符串
```

###### 6.颜色函数
> (1) hsl(色相，饱和度，明度)
```
sass语法：
background: hsl(0, 100, 50%);
css语法：
background: red;
```

>(2) adjust-hue()调整色相
```
sass语法：
$primary-color: #1265b6;
color: adjust-hue($primary-color, 137deg);
css语法：
color: #b61237;
```

>(3)lighten()、darken()调整颜色的明度
```
sass语法：
$base-color: hsl(222, 100%, 50%);
$light-color: lighten($base-color, 30%);
$dark-color: darken($base-color, 20%);

.alert-color {
  border: 1px solid $base-color;
  background-color: $light-color;
  color: $dark-color;
}

css语法：
.alert-color {
  border: 1px solid #004dff;
  background-color: #99b8ff;
  color: #002e99;
}
```

>(4)saturate()、desaturate()调整颜色的饱和度
```
sass语法：
$base-color: hsl(222, 50%, 50%);
$saturate: saturate($base-color, 50%);
$desaturate: desaturate($base-color, 30%);

.alert-color-ex {
  background-color: $saturate;
  color: $desaturate;
}

css语法：
.alert-color-ex {
  background-color: #004dff;
  color: #667599;
}
```

>(5)opacify()增加颜色不透明度、transparentize()减少颜色不透明度
```
sass语法：
$base-color: hsla(222, 50%, 50%, .5);
$fade-in-color: opacify($base-color, .3);
$fade-out-color: transparentize($base-color, .2);

.alert-color-fade {
  background-color: $fade-in-color;
  color: $fade-out-color;
}

css语法：
.alert-color-fade {
  background-color: rgba(64, 102, 191, 0.8);
  color: rgba(64, 102, 191, 0.3);
}
```

###### 7.列表函数(list)
>在sass中什么是列表？
sass中属性声明的一串值，可以用空格分隔开，也可以用逗号分隔开。
```
border: 1px solid #fff;(border属性的值就是一个列表)
font-family: Ahem!, sans-serif;(font-family的值就是一个列表)
```
>列表函数：
```
>>length(1px solid #fff)求列表的长度
3
>>nth(2px 3px 1px 4px, 3)求指定位置列表的值
1px
>>index(2px 1px solid #fff, solid)求列表指定值的位置
3
>>append(2px 1px, solid)往列表后插入一个值
(2px 1px solid)
>>join(1px, solid #fff)把两个列表结合(默认以空格结合)
(1px solid #fff)
>>join(1px 2px, 3px 4px, comma)把两个列表以逗号结合
(1px, 2px, 3px, 4px)
```

###### 8.map类型数据(跟list很相似)
>map数据格式(key1: value1, key2: value2, key3: value3)
注：用在列表类型数据上的方法同样可以用在map类型数据上
```
$colors: (light: #fff, dark: #000)
>>length($colors)
2
>>map-get($colors, light)获得map数据key指定的value值
#ffffff
>>map-get($colors, dark)
#000000
>>map-keys($colors)获得所有map数据的key值
("light", "dark")
>>map-values($colors)获得所有map数据的value值
(#ffffff, #000000)
>>map-has-key($colors, light)map数据是否有指定的key值，返回值为true或false
true
>>map-has-key($colors, darken)
false
>>map-merge($colors, (gray: #eee))合并map数据
(light: #fff, dark: #000, gray: #eee)
>>$colors: map-merge($colors, (gray: #eee))把合并后的数据赋给$colors
(light: #fff, dark: #000, gray: #eee)
>>map-remove($colors, dark)删除map数据指定的key: value
(light: #fff, gray: #eee)
```

###### 9.布尔值boolean
>and、or、not(且、或、非)
```
>> 5px < 3px
false
>> (5px > 3px) and (5px < 3px)且
false
>> (5px > 3px) or (5px < 3px)或
true
>> (5px > 3px) or (2px < 3px)
true
>> (5px > 3px) and (2px < 3px)
true
>> not(5px > 3px)非
false
>> not(5px < 3px)
true
```

###### 10.Interpolation
>#{变量名} 把一个值插入到另一个值里
```
sass语法：
$name: "info";
$attr: "border"

.alert-#{$name} {
  #{$attr}-color: #fff;
}

css语法：
.alert-info {
  border-color: #ffffff;
}
```