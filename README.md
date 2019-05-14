# lua 简明教程

本文主要来自 w3cschool，部分内容加以修改和补充。

Lua 中文交流 telegram 群： https://t.me/lua_cn

<!-- vim-markdown-toc GFM -->

- [基本语法](#基本语法)
  - [注释](#注释)
  - [标识符](#标识符)
  - [关键词](#关键词)
  - [全局变量](#全局变量)
- [数据类型](#数据类型)
  - [nil（空）](#nil空)
  - [boolean（布尔）](#boolean布尔)
  - [number（数字）](#number数字)
  - [string（字符串）](#string字符串)
  - [table（表）](#table表)
  - [function（函数）](#function函数)
  - [thread（线程）](#thread线程)
  - [userdata（自定义类型）](#userdata自定义类型)
- [Lua 变量](#lua-变量)
  - [赋值语句](#赋值语句)
  - [索引](#索引)
- [Lua 循环](#lua-循环)
  - [while 循环](#while-循环)
  - [for 循环](#for-循环)
    - [数值 for 循环](#数值-for-循环)
    - [泛型 for 循环](#泛型-for-循环)
  - [repeat until 循环](#repeat-until-循环)
  - [嵌套循环](#嵌套循环)
  - [循环控制语句](#循环控制语句)
- [Lua 流程控制](#lua-流程控制)
  - [if 语句](#if-语句)
  - [if else 语句](#if-else-语句)
  - [else if 语句](#else-if-语句)
  - [if else 嵌套](#if-else-嵌套)
- [Lua 函数](#lua-函数)
  - [多返回值](#多返回值)
  - [可变参数](#可变参数)
- [Lua 运算符](#lua-运算符)
  - [算术运算符](#算术运算符)
  - [关系运算符](#关系运算符)
- [Lua 字符串](#lua-字符串)
  - [字符串操作](#字符串操作)
- [Lua 数组](#lua-数组)
  - [一维数组](#一维数组)
  - [多维数组](#多维数组)
- [Lua 迭代器](#lua-迭代器)
  - [泛型 for 迭代器](#泛型-for-迭代器)
  - [无状态的迭代器](#无状态的迭代器)
- [Lua table(表)](#lua-table表)
  - [table(表)的构造](#table表的构造)
  - [Table 操作](#table-操作)
- [Lua 模块与包](#lua-模块与包)
  - [加载机制](#加载机制)

<!-- vim-markdown-toc -->

## 基本语法

Lua 提供了交互式编程模式和脚本式编程模式，交互式编程模式可以在命令行中直接输入代码并查看结果。
Lua 的交互式编程可通过 `lua -i` 或者 `lua` 命令开启。

```
Microsoft Windows [版本 6.1.7601]
版权所有 (c) 2009 Microsoft Corporation。保留所有权利。

F:\languages>lua
Lua 5.1.5  Copyright (C) 1994-2012 Lua.org, PUC-Rio
>
```

在命令行中，输入以下命令:

```
> print("hello world!")
```

按下回车后可以看到执行结果如下：

```
> print("hello world!")
hello world!
>
```

脚本式编程，指的是，将代码写入文件后并执行，比如新建 hello.lua 文件，并写入以下内容：

```lua
print("hello world!")
```

使用 lua 执行以上脚本，输出结果为：

```
F:\languages\lua>lua hello.lua
hello world!
```

我们也可以在 lua 文件顶部添加执行命令，比如，添加

```
#!/usr/local/bin/lua
print("hello world!")
```

然后通过 `./hello.lua` 的方式来执行。

### 注释

Lua 的注释分单行注释和多行注释，单行注释以两个减号开头

```lua
-- 这是一个单行注释
print("hello world!")
```

多行注释在段落前后分别有 `--[[` 和 `--]]`。

```lua
--[[
这是一个多行注释
这是一个多行注释
--]]
print("hello world!")
```

### 标识符

Lua 标识符用于定义一个变量、函数的名称。标示符以一个字母 A 到 Z 或 a 到 z 或下划线 \_ 开头后加上 0 个或多个字母，下划线，数字（0 到 9）。

最好不要使用下划线加大写字母的标示符，因为 Lua 的保留字也是这样的。

Lua 不允许使用特殊字符如 @, \$, 和 % 来定义标示符。

Lua 是一个区分大小写的编程语言。因此在 Lua 中 W3c 与 w3c 是两个不同的标示符。

以下列出了一些正确的标示符：

```
mohd         zara      abc     move_name    a_123
myname50     _temp     j       a23b9        retVal
```

### 关键词

Lua 本身一些保留的关键字是不可以使用的，比如循环用到的 for while 等。

以下列出了 Lua 的保留关键字。保留关键字不能作为常量或变量或其他用户自定义标示符：

```
| and      | break | do    | else   |
| elseif   | end   | false | for    |
| function | if    | in    | local  |
| nil      | not   | or    | repeat |
| return   | then  | true  | until  |
| while    |
```

### 全局变量

Lua 的默认情况下变量总是全局变量，全局变量也不需要声明，直接赋值即可，访问一个未赋值即不存在的全局变量，
也不会报错，只不过得到的是：nil

```
> print(b)
nil
> b=10
> print(b)
10
>
```

如果你想删除一个全局变量，只需要将变量赋值为 nil。

```
> b = nil
> print(b)
nil
```

这样变量 b 就好像从没被使用过一样。换句话说, 当且仅当一个变量不等于 nil 时，这个变量即存在。

## 数据类型

Lua 是动态类型语言，变量不要类型定义,只需要为变量赋值。 值可以存储在变量中，作为参数传递或结果返回。

Lua 中有 8 个基本类型分别为：nil、boolean、number、string、userdata、function、thread 和 table。

| 数据类型 | 描述                                                                                                                                                                                       |
| -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| nil      | 这个最简单，只有值 nil 属于该类，表示一个无效值（在条件表达式中相当于 false）。                                                                                                            |
| boolean  | 包含两个值：false 和 true。                                                                                                                                                                |
| number   | 表示双精度类型的实浮点数                                                                                                                                                                   |
| string   | 字符串由一对双引号或单引号来表示                                                                                                                                                           |
| function | 由 C 或 Lua 编写的函数                                                                                                                                                                     |
| userdata | 表示任意存储在变量中的 C 数据结构                                                                                                                                                          |
| thread   | 表示执行的独立线路，用于执行协同程序                                                                                                                                                       |
| table    | Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字或者是字符串。在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。 |

我们可以使用 type 函数测试给定变量或者值的类型

```lua
print(type("Hello world"))      --> string
print(type(10.4*3))             --> number
print(type(print))              --> function
print(type(type))               --> function
print(type(true))               --> boolean
print(type(nil))                --> nil
print(type(type(X)))            --> string
```

### nil（空）

nil 类型表示一种没有任何有效值，它只有一个值 -- nil，例如打印一个没有赋值的变量，便会输出一个 nil 值：

```
> print(type(a))
nil
>
```

对于全局变量和 table，nil 还有一个"删除"作用，给全局变量或者 table 表里的变量赋一个 nil 值，等同于把它们删掉，执行下面代码就知：

```lua
tab1 = { key1 = "val1", key2 = "val2", "val3" }
for k, v in pairs(tab1) do
    print(k .. " - " .. v)
end

tab1.key1 = nil
for k, v in pairs(tab1) do
    print(k .. " - " .. v)
end
```

![lua nil](https://user-images.githubusercontent.com/13142418/57571049-900a5700-743b-11e9-9691-e99f610d1769.png)

### boolean（布尔）

boolean 类型只有两个可选值：true（真） 和 false（假），Lua 把 false 和 nil 看作是"假"，其他的都为"真":

```lua
print(type(true))
print(type(false))
print(type(nil))

if type(false) or type(nil) then
    print("false and nil are false!")
else
    print("other is true!")
end
```

![luaboolean](https://user-images.githubusercontent.com/13142418/57571099-2dfe2180-743c-11e9-8737-bce16161cb88.png)

### number（数字）

Lua 默认只有一种 number 类型 -- double（双精度）类型（默认类型可以修改 luaconf.h 里的定义），以下几种写法都被看作是 number 类型：

```lua
print(type(2))
print(type(2.2))
print(type(0.2))
print(type(2e+1))
print(type(0.2e-1))
print(type(7.8263692594256e-06))
```

![luanumber](https://user-images.githubusercontent.com/13142418/57571118-6a318200-743c-11e9-868b-2501a4503440.png)

### string（字符串）

字符串由一对双引号或单引号来表示。

```lua
string1 = "this is string1"
string2 = 'this is string2'
```

也可以用 2 个方括号 "[[]]" 来表示"一块"字符串。

```lua
html = [[
<html>
<head></head>
<body>
    <a href="//www.w3cschool.cn/">w3cschoolW3Cschool教程</a>
</body>
</html>
]]
print(html)
```

![luastring](https://user-images.githubusercontent.com/13142418/57571136-ad8bf080-743c-11e9-95b7-6ef63f121761.png)

在对一个数字字符串上进行算术操作时，Lua 会尝试将这个数字字符串转成一个数字:

```
> print("2" + 6)
8.0
> print("2" + "6")
8.0
> print("2 + 6")
2 + 6
> print("-2e2" * "6")
-1200.0
> print("error" + 1)
stdin:1: attempt to perform arithmetic on a string value
stack traceback:
 stdin:1: in main chunk
    [C]: in ?
>
```

以上代码中"error" + 1 执行报错了，字符串连接使用的是 .. ，如：

```
> print("a" .. 'b')
ab
> print(157 .. 428)
157428
>
```

使用 # 来计算字符串的长度，放在字符串前面，如下实例：

```
> len = "www.w3cschool.cn"
> print(#len)
16
> print(#"www.w3cschool.cn")
16
>
```

### table（表）

在 Lua 里，table 的创建是通过"构造表达式"来完成，最简单构造表达式是{}，用来创建一个空表。也可以在表里添加一些数据，直接初始化表:

```lua
-- 创建一个空的 table
local tbl1 = {}

-- 直接初始表
local tbl2 = {"apple", "pear", "orange", "grape"}
```

Lua 中的表（table）其实是一个"关联数组"（associative arrays），数组的索引可以是数字或者是字符串。

```lua
-- table_test.lua 脚本文件
a = {}
a["key"] = "value"
key = 10
a[key] = 22
a[key] = a[key] + 11
for k, v in pairs(a) do
    print(k .. " : " .. v)
end
```

脚本执行结果为：

![luatable](https://user-images.githubusercontent.com/13142418/57571278-77e80700-743e-11e9-8129-64922e0f5844.png)

不同于其他语言的数组把 0 作为数组的初始索引，在 Lua 里表的默认初始索引一般以 1 开始。

```lua
-- table_test2.lua 脚本文件
local tbl = {"apple", "pear", "orange", "grape"}
for key, val in pairs(tbl) do
    print("Key", key)
end
```

脚本执行结果为：

![table](https://user-images.githubusercontent.com/13142418/57571292-98b05c80-743e-11e9-9870-0d1b29550caa.png)

table 不会固定长度大小，有新数据添加时 table 长度会自动增长，没初始的 table 都是 nil。

```lua
-- table_test3.lua 脚本文件
a3 = {}
for i = 1, 10 do
    a3[i] = i
end
a3["key"] = "val"
print(a3["key"])
print(a3["none"])
```

脚本执行结果为：

![table3](https://user-images.githubusercontent.com/13142418/57571301-cac1be80-743e-11e9-8cbe-fd5fd467f8ad.png)

### function（函数）

在 Lua 中，函数是被看作是"第一类值（First-Class Value）"，函数可以存在变量里:

```lua
-- function_test.lua 脚本文件
function factorial1(n)
    if n == 0 then
        return 1
    else
        return n * factorial1(n - 1)
    end
end
print(factorial1(5))
factorial2 = factorial1
print(factorial2(5))
```

脚本执行结果为：

![luafunction](https://user-images.githubusercontent.com/13142418/57571311-f0e75e80-743e-11e9-9da0-05391afc4db8.png)

function 可以以匿名函数（anonymous function）的方式通过参数传递:

```lua
-- function_test2.lua 脚本文件
function anonymous(tab, fun)
    for k, v in pairs(tab) do
        print(fun(k, v))
    end
end
tab = { key1 = "val1", key2 = "val2" }
anonymous(tab, function(key, val)
    return key .. " = " .. val
end)
```

脚本执行结果为：

![luafunction2](https://user-images.githubusercontent.com/13142418/57571319-0bb9d300-743f-11e9-920f-d6536eac6eec.png)

### thread（线程）

在 Lua 里，最主要的线程是协同程序（coroutine）。它跟线程（thread）差不多，拥有自己独立的栈、局部变量和指令指针，可以跟其他协同程序共享全局变量和其他大部分东西。

线程跟协程的区别：线程可以同时多个运行，而协程任意时刻只能运行一个，并且处于运行状态的协程只有被挂起（suspend）时才会暂停。

### userdata（自定义类型）

userdata 是一种用户自定义数据，用于表示一种由应用程序或 C/C++ 语言库所创建的类型，可以将任意 C/C++ 的任意数据类型的数据（通常是 struct 和 指针）存储到 Lua 变量中调用。

## Lua 变量

变量在使用前，必须在代码中进行声明，即创建该变量。编译程序执行代码之前编译器需要知道如何给语句变量开辟存储区，用于存储变量的值。

Lua 变量有三种类型：全局变量、局部变量、表中的域。

函数外的变量默认为全局变量，除非用 local 显示声明。函数内变量与函数的参数默认为局部变量。

局部变量的作用域为从声明位置开始到所在语句块结束（或者是直到下一个同名局部变量的声明）。

变量的默认值均为 nil。

```lua
-- test.lua 文件脚本
a = 5               -- 全局变量
local b = 5         -- 局部变量

function joke()
    c = 5           -- 全局变量
    local d = 6     -- 局部变量
end

joke()
print(c,d)          --> 5 nil

do
    local a = 6     -- 局部变量
    b = 6           -- 全局变量
    print(a,b);     --> 6 6
end

print(a,b)      --> 5 6
```

以上脚本运行输出结果为：

![testlua](https://user-images.githubusercontent.com/13142418/57571437-e7f78c80-7440-11e9-9fcd-01db063616e5.png)

### 赋值语句

赋值是改变一个变量的值和改变表域的最基本的方法。

```lua
a = "hello" .. "world"
t = { n = 1, m = 2}
t.n = t.n + 1
print(t.n)
```

Lua 可以对多个变量同时赋值，变量列表和值列表的各个元素用逗号分开，赋值语句右边的值会依次赋给左边的变量。

```lua
a, b = 10, 2*x       -->       a=10; b=2*x
```

遇到赋值语句 Lua 会先计算右边所有的值然后再执行赋值操作，所以我们可以这样进行交换变量的值：

```lua
x, y = y, x                     -- swap 'x' for 'y'
a[i], a[j] = a[j], a[i]         -- swap 'a[i]' for 'a[i]'
```

当变量个数和值的个数不一致时，Lua 会一直以变量个数为基础采取以下策略：

```
a. 变量个数 > 值的个数             按变量个数补足nil
b. 变量个数 < 值的个数             多余的值会被忽略
```

例如：

```lua
a, b, c = 0, 1
print(a,b,c)             --> 0   1   nil

a, b = a+1, b+1, b+2     -- value of b+2 is ignored
print(a,b)               --> 1   2

a, b, c = 0
print(a,b,c)             --> 0   nil   nil
```

上面最后一个例子是一个常见的错误情况，注意：如果要对多个变量赋值必须依次对每个变量赋值。

```lua
a, b, c = 0, 0, 0
print(a,b,c)             --> 0   0   0
```

多值赋值经常用来交换变量，或将函数调用返回给变量：

```lua
a, b = f()
```

f()返回两个值，第一个赋给 a，第二个赋给 b。

应该尽可能的使用局部变量，有两个好处：

- 避免命名冲突。
- 访问局部变量的速度比全局变量更快。

### 索引

对 table 的索引使用方括号 []。Lua 也提供了 . 操作。

```lua
t[i]
t.i                 -- 当索引为字符串类型时的一种简化写法
gettable_event(t,i) -- 采用索引访问本质上是一个类似这样的函数调用
```

例如：

```
> site = {}
> site["key"] = "www.w3cschool.cn"
> print(site["key"])
www.w3cschool.cn
> print(site.key)
www.w3cschool.cn
```

## Lua 循环

很多情况下我们需要做一些有规律性的重复操作，因此在程序中就需要重复执行某些语句。

一组被重复执行的语句称之为循环体，能否继续重复，决定循环的终止条件。

循环结构是在一定条件下反复执行某段程序的流程结构，被反复执行的程序被称为循环体。

循环语句是由循环体及循环的终止条件两部分组成的。

Lua 语言提供了以下几种循环处理方式：

| 循环类型           | 描述                                                                            |
| ------------------ | ------------------------------------------------------------------------------- |
| while 循环         | 在条件为 true 时，让程序重复地执行某些语句。执行语句前会先检查条件是否为 true。 |
| for 循环           | 重复执行指定语句，重复次数可在 for 语句中控制。                                 |
| Lua repeat...until | 重复执行循环，直到 指定的条件为真时为止                                         |
| 循环嵌套           | 可以在循环内嵌套一个或多个循环语句（while、for、do..while）                     |

### while 循环

```
while(condition)
do
   statements
end
```

statements(循环体语句) 可以是一条或多条语句，condition(条件) 可以是任意表达式，在 condition(条件) 为 true 时执行循环体语句。

示例：

```lua
a = 15
while ( a < 20 )
do
   print("a 的值为:", a)
   a = a + 1
end
```

![whileloop](https://user-images.githubusercontent.com/13142418/57582840-49306600-74fc-11e9-82e9-9d75b35c7cb5.png)

### for 循环

Lua 编程语言中 for 循环语句可以重复执行指定语句，重复次数可在 for 语句中控制。

Lua 编程语言中 for 语句有两大类：：

- 数值 for 循环
- 泛型 for 循环

#### 数值 for 循环

基本语法：

```log
for var=exp1,exp2,exp3 do
    <执行体>
end
```

var 从 exp1 变化到 exp2，每次变化以 exp3 为步长递增 var，并执行一次"执行体"。exp3 是可选的，如果不指定，默认为 1。

实例：

```lua
for i=1,f(x) do
    print(i)
end

for i=10,1,-1 do
    print(i)
end
```

for 的三个表达式在循环开始前一次性求值，以后不再进行求值。比如上面的 f(x)只会在循环开始前执行一次，其结果用在后面的循环中。

验证如下:

```lua
function f(x)
    print("方法被执行")
    return x*2
end
for i=1,f(5) do print(i)
end
```

#### 泛型 for 循环

泛型 for 循环通过一个迭代器函数来遍历所有值，类似 java 中的 foreach 语句。

Lua 编程语言中泛型 for 循环语法格式:

```lua
--打印数组a的所有值
for i,v in ipairs(a)
    do print(v)
end
```

示例：

```lua
days = {"Suanday","Monday","Tuesday"}
for i,v in ipairs(days) do  print(v) end
```

### repeat until 循环

Lua 编程语言中 repeat...until 循环语句不同于 for 和 while 循环，for 和 while 循环的
条件语句在当前循环执行开始时判断，而 repeat...until 循环的条件语句在当前循环结束后判断。

基本语法格式：

```txt
repeat
   statements
until( condition )
```

我们注意到循环条件判断语句（condition）在循环体末尾部分，所以在条件进行判断前循环体都会执行一次。
如果条件判断语句（condition）为 false，循环会重新开始执行，直到条件判断语句（condition）为 true 才会停止执行。

示例代码：

```lua
--[ 变量定义 --]
a = 10
--[ 执行循环 --]
repeat
   print("a的值为:", a)
   a = a + 1
until( a > 13 )
```

允许结果如下：

![repeat](https://user-images.githubusercontent.com/13142418/57583373-65cf9c80-7502-11e9-821e-e8f929aaad45.png)

### 嵌套循环

Lua 编程语言中允许循环中嵌入循环。以下实例演示了 Lua 循环嵌套的应用。
语法

Lua 编程语言中 for 循环嵌套语法格式:

```
for init,max/min value, increment
do
   for init,max/min value, increment
   do
      statements
   end
   statements
end
```

Lua 编程语言中 while 循环嵌套语法格式:

```
while(condition)
do
   while(condition)
   do
      statements
   end
   statements
end
```

Lua 编程语言中 repeat...until 循环嵌套语法格式:

```
repeat
   statements
   repeat
      statements
   until( condition )
until( condition )
```

除了以上同类型循环嵌套外，我们还可以使用不同的循环类型来嵌套，如 for 循环体中嵌套 while 循环。
实例

以下实例使用了 for 循环嵌套:

```lua
j =2
for i=2,10 do
   for j=2,(i/j) , 2 do
      if(not(i%j))
      then
         break
      end
      if(j > (i/j))then
         print("i 的值为：",i)
      end
   end
end
```

运行结果如下：

![luawhilefor](https://user-images.githubusercontent.com/13142418/57583680-703f6580-7505-11e9-82d3-581b024f58ad.png)

### 循环控制语句

lua 支持 break 语句，退出当前循环或语句，并开始脚本执行紧接着的语句:

```lua
--[ 定义变量 --]
a = 10

--[ while 循环 --]
while ( a < 20 )
do
   print("a 的值为:", a)
   a = a + 1
   if ( a > 13)
   then
      --[ 使用 break 语句终止循环 --]
      break
   end
end
```

## Lua 流程控制

Lua 编程语言流程控制语句通过程序设定一个或多个条件语句来设定。在条件为 true 时执行指定程序代码，在条件为 false 时执行其他指定代码。

控制结构的条件表达式结果可以是任何值，Lua 认为 false 和 nil 为假，true 和非 nil 为真。

要注意的是 Lua 中 0 为 true：

```lua
--[ 0 为true ]
if(0)
then
    print("0 为真")
end
```

以上代码输出结果为：

```
0 为真
```

Lua 提供了以下控制结构语句：

| 语句           | 描述                                                                              |
| -------------- | --------------------------------------------------------------------------------- |
| if 语句        | if 语句 由一个布尔表达式作为条件判断，其后紧跟其他语句组成。                      |
| if...else 语句 | if 语句 可以与 else 语句搭配使用, 在 if 条件表达式为 false 时执行 else 语句代码。 |
| if 嵌套语句    | 你可以在 if 或 else if 中使用一个或多个 if 或 else if 语句 。                     |

### if 语句

Lua if 语句 由一个布尔表达式作为条件判断，其后紧跟其他语句组成。

Lua if 语句语法格式如下：

```
if(布尔表达式)
then
   --[ 在布尔表达式为 true 时执行的语句 --]
end
```

在布尔表达式为 true 时会 if 中的代码块会被执行，在布尔表达式为 false 时，紧跟在 if 语句 end 之后的代码会被执行。

Lua 认为 false 和 nil 为假，true 和非 nil 为真。要注意的是 Lua 中 0 为 true。

实例

以下实例用于判断变量 a 的值是否小于 20：

```lua
--[ 定义变量 --]
a = 10

--[ 使用 if 语句 --]
if( a < 20 )
then
   --[ if 条件为 true 时打印以下信息 --]
   print("a 小于 20" )
end
print("a 的值为:", a)
```

以上代码执行结果如下：

![if](https://user-images.githubusercontent.com/13142418/57591306-4ddc3500-7563-11e9-90b7-a477f11add54.png)

### if else 语句

Lua if 语句可以与 else 语句搭配使用, 在 if 条件表达式为 false 时执行 else 语句代码块。

Lua if...else 语句语法格式如下：

```
if(布尔表达式)
then
   --[ 布尔表达式为 true 时执行该语句块 --]
else
   --[ 布尔表达式为 false 时执行该语句块 --]
end
```

在布尔表达式为 true 时会 if 中的代码块会被执行，在布尔表达式为 false 时，else 的代码块会被执行。

Lua 认为 false 和 nil 为假，true 和非 nil 为真。要注意的是 Lua 中 0 为 true。

实例

以下实例用于判断变量 a 的值：

```lua
--[ 定义变量 --]
a = 100;
--[ 检查条件 --]
if( a < 20 )
then
   --[ if 条件为 true 时执行该语句块 --]
   print("a 小于 20" )
else
   --[ if 条件为 false 时执行该语句块 --]
   print("a 大于 20" )
end
print("a 的值为 :", a)
```

以上代码执行结果如下：

### else if 语句

Lua if 语句可以与 else if...else 语句搭配使用, 在 if 条件表达式为 false 时执行 else if...else 语句代码块，用于检测多个条件语句。

Lua if...else if...else 语句语法格式如下：

```
if( 布尔表达式 1)
then
   --[ 在布尔表达式 1 为 true 时执行该语句块 --]

else if( 布尔表达式 2)
   --[ 在布尔表达式 2 为 true 时执行该语句块 --]

else if( 布尔表达式 3)
   --[ 在布尔表达式 3 为 true 时执行该语句块 --]
else
   --[ 如果以上布尔表达式都不为 true 则执行该语句块 --]
end
```

示例代码，以下示例代码对变量 a 值进行判断：

```lua
--[ 定义变量 --]
a = 100

--[ 检查布尔条件 --]
if( a == 10 )
then
   --[ 如果条件为 true 打印以下信息 --]
   print("a 的值为 10" )
elseif( a == 20 )
then
   --[ if else if 条件为 true 时打印以下信息 --]
   print("a 的值为 20" )
elseif( a == 30 )
then
   --[ if else if condition 条件为 true 时打印以下信息 --]
   print("a 的值为 30" )
else
   --[ 以上条件语句没有一个为 true 时打印以下信息 --]
   print("没有匹配 a 的值" )
end
print("a 的真实值为: ", a )
```

运行结果如下：

![elseif](https://user-images.githubusercontent.com/13142418/57591705-4d449e00-7565-11e9-97dd-a6c4c667597b.png)

### if else 嵌套

Lua if 语句允许嵌套, 这就意味着你可以在一个 if 或 else if 语句中插入其他的 if 或 else if 语句。
Lua if 嵌套语句语法格式如下：

```
if( 布尔表达式 1)
then
   --[ 布尔表达式 1 为 true 时执行该语句块 --]
   if(布尔表达式 2)
   then
      --[ 布尔表达式 2 为 true 时执行该语句块 --]
   end
end
```

你可以用同样的方式嵌套 else if...else 语句。

实例

以下实例用于判断变量 a 和 b 的值：

```lua
--[ 定义变量 --]
a = 100;
b = 200;

--[ 检查条件 --]
if( a == 100 )
then
   --[ if 条件为 true 时执行以下 if 条件判断 --]
   if( b == 200 )
   then
      --[ if 条件为 true 时执行该语句块 --]
      print("a 的值为 100 b 的值为 200" );
   end
end
print("a 的值为 :", a );
print("b 的值为 :", b );
```

以上代码执行结果如下：

![ififif](https://user-images.githubusercontent.com/13142418/57592594-1d4bc980-756a-11e9-894b-fe6137424a6d.png)

## Lua 函数

在 Lua 中，函数是对语句和表达式进行抽象的主要方法。既可以用来处理一些特殊的工作，也可以用来计算一些值。

Lua 提供了许多的内建函数，你可以很方便的在程序中调用它们，如 print()函数可以将传入的参数打印在控制台上。

Lua 函数主要有两种用途：

1. 完成指定的任务，这种情况下函数作为调用语句使用；
2. 计算并返回值，这种情况下函数作为赋值语句的表达式使用。

函数定义

Lua 编程语言函数定义格式如下：

```
optional_function_scope function function_name( argument1, argument2, argument3..., argumentn)
   function_body
 return result_params_comma_separated
end
```

解析：

- `optional_function_scope`: 该参数是可选的制定函数是全局函数还是局部函数，未设置该参数末尾为全局函数，如果你需要设置函数为局部函数需要使用关键字 local。
- `function_name`: 指定函数名称。
- `argument1, argument2, argument3..., argumentn` : 函数参数，多个参数以逗号隔开，函数也可以不带参数。
- `function_body`: 函数体，函数中需要执行的代码语句块。
- `result_params_comma_separated`: 函数返回值，Lua 语言函数可以返回多个值，每个值以逗号隔开。

实例

以下实例定义了函数 max()，参数为 num1, num2，用于比较两值的大小，并返回最大值：

```lua
--[[ 函数返回两个值的最大值 --]]
function max(num1, num2)

   if (num1 > num2) then
      result = num1;
   else
      result = num2;
   end

   return result;
end
-- 调用函数
print("两值比较最大值为 ",max(10,4))
print("两值比较最大值为 ",max(5,6))
```

以上代码执行结果为：

![max](https://user-images.githubusercontent.com/13142418/57593287-5174b980-756d-11e9-867c-a1754ec68988.png)

Lua 中我们可以将函数作为参数传递给函数，如下实例：

```lua
myprint = function(param)
   print("这是打印函数 -   ##",param,"##")
end

function add(num1,num2,functionPrint)
   result = num1 + num2
   -- 调用传递的函数参数
   functionPrint(result)
end
myprint(10)
-- myprint 函数作为参数传递
add(2,5,myprint)
```

以上代码执行结果为：

![function](https://user-images.githubusercontent.com/13142418/57593302-63565c80-756d-11e9-9e55-f8dc2177a253.png)

### 多返回值

Lua 函数可以返回多个结果值，比如 string.find，其返回匹配串"开始和结束的下标"（如果不存在匹配串返回 nil）。

```
> s, e = string.find("www.w3cschool.cn", "w3cschool")
> print(s, e)
5	13
```

Lua 函数中，在 return 后列出要返回的值得列表即可返回多值，如：

```lua
function maximum (a)
    local mi = 1             -- 最大值索引
    local m = a[mi]          -- 最大值
    for i,val in ipairs(a) do
       if val > m then
           mi = i
           m = val
       end
    end
    return m, mi
end

print(maximum({8,10,23,12,5}))
```

以上代码执行结果为：

![maximum](https://user-images.githubusercontent.com/13142418/57593257-373adb80-756d-11e9-8b28-d578035cf2aa.png)

### 可变参数

Lua 函数可以接受可变数目的参数，和 C 语言类似在函数参数列表中使用三点（...) 表示函数有可变的参数。

Lua 将函数的参数放在一个叫 arg 的表中，#arg 表示传入参数的个数。

例如，我们计算几个数的平均值：

```lua
function average(...)
   result = 0
   local arg={...}
   for i,v in ipairs(arg) do
      result = result + v
   end
   print("总共传入 " .. #arg .. " 个数")
   return result/#arg
end

print("平均值为",average(10,5,3,4,5,6))
```

以上代码执行结果为：

![average](https://user-images.githubusercontent.com/13142418/57593230-1bcfd080-756d-11e9-98b1-e4f515980e6d.png)

## Lua 运算符

运算符是一个特殊的符号，用于告诉解释器执行特定的数学或逻辑运算。Lua 提供了以下几种运算符类型：

- 算术运算符
- 关系运算符
- 逻辑运算符
- 其他运算符

### 算术运算符

下表列出了 Lua 语言中的常用算术运算符，设定 A 的值为 10，B 的值为 20：

| 操作符 | 描述 | 实例                 |
| ------ | ---- | -------------------- |
| `+`    | 加法 | `A + B` 输出结果 30  |
| `-`    | 减法 | `A - B` 输出结果 -10 |
| `*`    | 乘法 | `A * B` 输出结果 200 |
| `/`    | 除法 | `B / A` 输出结果 2   |
| `%`    | 取余 | `B % A` 输出结果 0   |
| `^`    | 乘幂 | `A ^ 2` 输出结果 100 |
| `-`    | 负号 | `-A` 输出结果 -10    |

实例

我们可以通过以下实例来更加透彻的理解算术运算符的应用：

```lua
a = 21
b = 10
c = a + b
print("Line 1 - c 的值为 ", c )
c = a - b
print("Line 2 - c 的值为 ", c )
c = a * b
print("Line 3 - c 的值为 ", c )
c = a / b
print("Line 4 - c 的值为 ", c )
c = a % b
print("Line 5 - c 的值为 ", c )
c = a^2
print("Line 6 - c 的值为 ", c )
c = -a
print("Line 7 - c 的值为 ", c )
```

以上程序执行结果为：

```
Line 1 - c 的值为 31
Line 2 - c 的值为 11
Line 3 - c 的值为 210
Line 4 - c 的值为 2.1
Line 5 - c 的值为 1
Line 6 - c 的值为 441
Line 7 - c 的值为 -21
```

### 关系运算符

下表列出了 Lua 语言中的常用关系运算符，设定 A 的值为 10，B 的值为 20：

| 操作符 | 描述                                                               | 实例                  |
| ------ | ------------------------------------------------------------------ | --------------------- |
| ==     | 等于，检测两个值是否相等，相等返回 true，否则返回 false            | (A == B) 为 false。   |
| ~=     | 不等于，检测两个值是否相等，相等返回 false，否则返回 true          | (A ~= B) 为 true。    |
| >      | 大于，如果左边的值大于右边的值，返回 true，否则返回 false          | (A > B) 为 false。    |
| <      | 小于，如果左边的值大于右边的值，返回 false，否则返回 true          | (A < B) 为 true。     |
| >=     | 大于等于，如果左边的值大于等于右边的值，返回 true，否则返回 false  | (A >= B) is not true. |
| <=     | 小于等于， 如果左边的值小于等于右边的值，返回 true，否则返回 false | (A <= B) is true.     |

实例

我们可以通过以下实例来更加透彻的理解关系运算符的应用：

```lua
a = 21
b = 10

if( a == b )
then
   print("Line 1 - a 等于 b" )
else
   print("Line 1 - a 不等于 b" )
end

if( a ~= b )
then
   print("Line 2 - a 不等于 b" )
else
   print("Line 2 - a 等于 b" )
end

if ( a < b )
then
   print("Line 3 - a 小于 b" )
else
   print("Line 3 - a 大于等于 b" )
end

if ( a > b )
then
   print("Line 4 - a 大于 b" )
else
   print("Line 5 - a 小于等于 b" )
end

-- 修改 a 和 b 的值
a = 5
b = 20
if ( a <= b )
then
   print("Line 5 - a 小于等于  b" )
end

if ( b >= a )
then
   print("Line 6 - b 大于等于 a" )
end
```

以上程序执行结果为：

```
Line 1 - a 不等于 b
Line 2 - a 不等于 b
Line 3 - a 大于等于 b
Line 4 - a 大于 b
Line 5 - a 小于等于 b
Line 6 - b 大于等于 a
```

## Lua 字符串

字符串或串(String)是由数字、字母、下划线组成的一串字符。

Lua 语言中字符串可以使用以下三种方式来表示：

- 单引号间的一串字符。
- 双引号间的一串字符。
- [[和]]间的一串字符。

以上三种方式的字符串实例如下：

```lua
string1 = "Lua"
print("\"字符串 1 是\"", string1)
string2 = 'w3cschool.cn'
print("字符串 2 是", string2)


string3 = [["Lua 教程"]]
print("字符串 3 是", string3)
```

以上代码执行输出结果为：

```
"字符串 1 是" Lua
字符串 2 是    w3cschool.cn
字符串 3 是   "Lua 教程"
```

转义字符用于表示不能直接显示的字符，比如后退键，回车键，等。如在字符串转换双引号可以使用 "\""。

所有的转义字符和所对应的意义：

| 转义字符     | 意义                                 | ASCII 码值（十进制） |
| ------------ | ------------------------------------ | -------------------- |
| \a           | 响铃(BEL)                            | 007                  |
| \b           | 退格(BS) ，将当前位置移到前一列      | 008                  |
| \f           | 换页(FF)，将当前位置移到下页开头     | 012                  |
| \n           | 换行(LF) ，将当前位置移到下一行开头  | 010                  |
| \r           | 回车(CR) ，将当前位置移到本行开头    | 013                  |
| \t           | 水平制表(HT) （跳到下一个 TAB 位置） | 009                  |
| \v           | 垂直制表(VT)                         | 011                  |
| \\           | 代表一个反斜线字符''\'               | 092                  |
| \'           | 代表一个单引号（撇号）字符           | 039                  |
| \"           | 代表一个双引号字符                   | 034                  |
| 空字符(NULL) | 000                                  |
| \ddd         | 1 到 3 位八进制数所代表的任意字符    | 三位八进制           |
| \xhh         | 1 到 2 位十六进制所代表的任意字符    | 二位十六进制         |


### 字符串操作

Lua 提供了很多的方法来支持字符串的操作：

字符串大小写转换

以下实例演示了如何对字符串大小写进行转换：

```lua
string1 = "Lua";
print(string.upper(string1))
print(string.lower(string1))
```

以上代码执行结果为：

```
LUA
lua
```

字符串查找与反转

以下实例演示了如何对字符串进行查找与反转操作：


```lua
string = "Lua Tutorial"
-- 查找字符串
print(string.find(string,"Tutorial"))
reversedString = string.reverse(string)
print("新字符串为",reversedString)
```

以上代码执行结果为：

```
5    12
新字符串为   lairotuT auL
```

字符串格式化

以下实例演示了如何对字符串进行格式化操作：

```lua
string1 = "Lua"
string2 = "Tutorial"
number1 = 10
number2 = 20
-- 基本字符串格式化
print(string.format("基本格式化 %s %s",string1,string2))
-- 日期格式化
date = 2; month = 1; year = 2014
print(string.format("日期格式化 %02d/%02d/%03d", date, month, year))
-- 十进制格式化
print(string.format("%.4f",1/3))
```

以上代码执行结果为：

```
基本格式化 Lua Tutorial
日期格式化 02/01/2014
0.3333
```

字符与整数相互转换

以下实例演示了字符与整数相互转换：

```lua
-- 字符转换
-- 转换第一个字符
print(string.byte("Lua"))
-- 转换第三个字符
print(string.byte("Lua",3))
-- 转换末尾第一个字符
print(string.byte("Lua",-1))
-- 第二个字符
print(string.byte("Lua",2))
-- 转换末尾第二个字符
print(string.byte("Lua",-2))


-- 整数 ASCII 码转换为字符
print(string.char(97))
```

以上代码执行结果为：

```
76
97
97
117
117
a
```


其他常用函数

以下实例演示了其他字符串操作，如计算字符串长度，字符串连接，字符串复制等：

```lua
string1 = "www."
string2 = "w3cschool"
string3 = ".cn"
-- 使用 .. 进行字符串连接
print("连接字符串",string1..string2..string3)


-- 字符串长度
print("字符串长度 ",string.len(string2))


-- 字符串复制 2 次
repeatedString = string.rep(string2,2)
print(repeatedString)
```

以上代码执行结果为：

```
连接字符串     www.w3cschool.cn
字符串长度     9
w3cschoolw3cschool
```

## Lua 数组

数组，就是相同数据类型的元素按一定顺序排列的集合，可以是一维数组和多维数组。

Lua 数组的索引键值可以使用整数表示，数组的大小不是固定的。

### 一维数组

一维数组是最简单的数组，其逻辑结构是线性表。一维数组可以用for循环出数组中的元素，如下实例：

```lua
array = {"Lua", "Tutorial"}

for i= 0, 2 do
   print(array[i])
end
```

以上代码执行输出结果为：

```
nil
Lua
Tutorial
```

正如你所看到的，我们可以使用整数索引来访问数组元素，如果知道的索引没有值则返回nil。

在 Lua 索引值是以 1 为起始，但你也可以指定 0 开始。

除此外我们还可以以负数为数组索引值：

```lua
array = {}

for i= -2, 2 do
   array[i] = i *2
end

for i = -2,2 do
   print(array[i])
end
```

以上代码执行输出结果为：

```
-4
-2
0
2
4
```

### 多维数组

多维数组即数组中包含数组或一维数组的索引键对应一个数组。

以下是一个三行三列的阵列多维数组：

```lua
-- 初始化数组
array = {}
for i=1,3 do
   array[i] = {}
      for j=1,3 do
         array[i][j] = i*j
      end
end

-- 访问数组
for i=1,3 do
   for j=1,3 do
      print(array[i][j])
   end
end
```

以上代码执行输出结果为：

```
1
2
3
2
4
6
3
6
9
```

不同索引键的三行三列阵列多维数组：

```lua
-- 初始化数组
array = {}
maxRows = 3
maxColumns = 3
for row=1,maxRows do
   for col=1,maxColumns do
      array[row*maxColumns +col] = row*col
   end
end

-- 访问数组
for row=1,maxRows do
   for col=1,maxColumns do
      print(array[row*maxColumns +col])
   end
end
```

以上代码执行输出结果为：

```
1
2
3
2
4
6
3
6
9
```

正如你所看到的，以上的实例中，数组设定了指定的索引值，这样可以避免出现 nil 值，有利于节省内存空间。

## Lua 迭代器

迭代器（iterator）是一种对象，它能够用来遍历标准模板库容器中的部分或全部元素，每个迭代器对象代表容器中的确定的地址

在Lua中迭代器是一种支持指针类型的结构，它可以遍历集合的每一个元素。

### 泛型 for 迭代器

泛型 for 在自己内部保存迭代函数，实际上它保存三个值：迭代函数、状态常量、控制变量。

泛型 for 迭代器提供了集合的 key/value 对，语法格式如下：

```
for k, v in pairs(t) do
    print(k, v)
end
```

上面代码中，k, v为变量列表；pairs(t)为表达式列表。

查看以下实例:

```lua
array = {"Lua", "Tutorial"}

for key,value in ipairs(array) 
do
   print(key, value)
end
```

以上代码执行输出结果为：

```
1  Lua
2  Tutorial
```

以上实例中我们使用了 Lua 默认提供的迭代函数 ipairs。

下面我们看看范性for的执行过程：

- 首先，初始化，计算in后面表达式的值，表达式应该返回范性for需要的三个值：迭代函数、状态常量、控制变量；与多值赋值一样，如果表达式返回的结果个数不足三个会自动用nil补足，多出部分会被忽略。
- 第二，将状态常量和控制变量作为参数调用迭代函数（注意：对于for结构来说，状态常量没有用处，仅仅在初始化时获取他的值并传递给迭代函数）。
- 第三，将迭代函数返回的值赋给变量列表。
- 第四，如果返回的第一个值为nil循环结束，否则执行循环体。
- 第五，回到第二步再次调用迭代函数

在Lua中我们常常使用函数来描述迭代器，每次调用该函数就返回集合的下一个元素。Lua 的迭代器包含以下两种类型：

- 无状态的迭代器
- 多状态的迭代器

### 无状态的迭代器

无状态的迭代器是指不保留任何状态的迭代器，因此在循环中我们可以利用无状态迭代器避免创建闭包花费额外的代价。

每一次迭代，迭代函数都是用两个变量（状态常量和控制变量）的值作为参数被调用，一个无状态的迭代器只利用这两个值可以获取下一个元素。

这种无状态迭代器的典型的简单的例子是ipairs，他遍历数组的每一个元素。

以下实例我们使用了一个简单的函数来实现迭代器，实现 数字 n 的平方：

```lua
function square(iteratorMaxCount,currentNumber)
   if currentNumber<iteratorMaxCount
   then
      currentNumber = currentNumber+1
   return currentNumber, currentNumber*currentNumber
   end
end

for i,n in square,3,0
do
   print(i,n)
end
```

以上实例输出结果为：

```
1	1
2	4
3	9
```

迭代的状态包括被遍历的表（循环过程中不会改变的状态常量）和当前的索引下标（控制变量），ipairs和迭代函数都很简单，我们在Lua中可以这样实现：

```lua
function iter (a, i)
    i = i + 1
    local v = a[i]
    if v then
       return i, v
    end
end
 
function ipairs (a)
    return iter, a, 0
end
```

当Lua调用ipairs(a)开始循环时，他获取三个值：迭代函数iter、状态常量a、控制变量初始值0；然后Lua调用iter(a,0)返回1,a[1]（除非a[1]=nil）；第二次迭代调用iter(a,1)返回2,a[2]……直到第一个nil元素。
多状态的迭代器

很多情况下，迭代器需要保存多个状态信息而不是简单的状态常量和控制变量，最简单的方法是使用闭包，还有一种方法就是将所有的状态信息封装到table内，将table作为迭代器的状态常量，因为这种情况下可以将所有的信息存放在table内，所以迭代函数通常不需要第二个参数。

以下实例我们创建了自己的迭代器：

```lua
array = {"Lua", "Tutorial"}

function elementIterator (collection)
   local index = 0
   local count = #collection
   -- 闭包函数
   return function ()
      index = index + 1
      if index <= count
      then
         --  返回迭代器的当前元素
         return collection[index]
      end
   end
end

for element in elementIterator(array)
do
   print(element)
end
```

以上实例输出结果为：

```
Lua
Tutorial
```

以上实例中我们可以看到，elementIterator 内使用了闭包函数，实现计算集合大小并输出各个元素。

## Lua table(表)

table 是 Lua 的一种数据结构用来帮助我们创建不同的数据类型，如：数字、字典等。

Lua table 使用关联型数组，你可以用任意类型的值来作数组的索引，但这个值不能是 nil。

Lua table 是不固定大小的，你可以根据自己需要进行扩容。

Lua也是通过table来解决模块（module）、包（package）和对象（Object）的。 例如string.format表示使用"format"来索引table string。

### table(表)的构造

构造器是创建和初始化表的表达式。表是Lua特有的功能强大的东西。最简单的构造函数是{}，用来创建一个空表。可以直接初始化数组:

```lua
-- 初始化表
mytable = {}

-- 指定值
mytable[1]= "Lua"

-- 移除引用
mytable = nil
-- lua 垃圾回收会释放内存
```

当我们为 table a 并设置元素，然后将 a 赋值给 b，则 a 与 b 都指向同一个内存。如果 a 设置为 nil ，则 b 同样能访问 table 的元素。如果没有指定的变量指向a，Lua的垃圾回收机制会清理相对应的内存。

以下实例演示了以上的描述情况：

```lua
-- 简单的 table
mytable = {}
print("mytable 的类型是 ",type(mytable))

mytable[1]= "Lua"
mytable["wow"] = "修改前"
print("mytable 索引为 1 的元素是 ", mytable[1])
print("mytable 索引为 wow 的元素是 ", mytable["wow"])

-- alternatetable和mytable的是指同一个 table
alternatetable = mytable

print("alternatetable 索引为 1 的元素是 ", alternatetable[1])
print("mytable 索引为 wow 的元素是 ", alternatetable["wow"])

alternatetable["wow"] = "修改后"

print("mytable 索引为 wow 的元素是 ", mytable["wow"])

-- 释放变量
alternatetable = nil
print("alternatetable 是 ", alternatetable)

-- mytable 仍然可以访问
print("mytable 索引为 wow 的元素是 ", mytable["wow"])

mytable = nil
print("mytable 是 ", mytable)
```

以上代码执行结果为：

```
mytable 的类型是 	table
mytable 索引为 1 的元素是 	Lua
mytable 索引为 wow 的元素是 	修改前
alternatetable 索引为 1 的元素是 	Lua
mytable 索引为 wow 的元素是 	修改前
mytable 索引为 wow 的元素是 	修改后
alternatetable 是 	nil
mytable 索引为 wow 的元素是 	修改后
mytable 是 	nil
```

### Table 操作


以下列出了 Table 操作常用的方法：

1. `table.concat (table [, step [, start [, end]]])`: concat是concatenate(连锁, 连接)的缩写. table.concat()函数列出参数中指定table的数组部分从start位置到end位置的所有元素, 元素间以指定的分隔符(sep)隔开。
2. `table.insert (table, [pos,] value)`: 在table的数组部分指定位置(pos)插入值为value的一个元素. pos参数可选, 默认为数组部分末尾.
3. `table.maxn (table)`: 指定table中所有正数key值中最大的key值. 如果不存在key值为正数的元素, 则返回0。(Lua5.2之后该方法已经不存在了,本文使用了自定义函数实现)
4. `table.remove (table [, pos])`: 返回table数组部分位于pos位置的元素. 其后的元素会被前移. pos参数可选, 默认为table长度, 即从最后一个元素删起。
5. `table.sort (table [, comp])`: 对给定的table进行升序排序。

接下来我们来看下这几个方法的实例。

Table 连接

我们可以使用 concat() 方法来连接两个 table:

```lua
fruits = {"banana","orange","apple"}
-- 返回 table 连接后的字符串
print("连接后的字符串 ",table.concat(fruits))

-- 指定连接字符
print("连接后的字符串 ",table.concat(fruits,", "))

-- 指定索引来连接 table
print("连接后的字符串 ",table.concat(fruits,", ", 2,3))
```

执行以上代码输出结果为：

```
连接后的字符串 	bananaorangeapple
连接后的字符串 	banana, orange, apple
连接后的字符串 	orange, apple
```

插入和移除

以下实例演示了 table 的插入和移除操作:

```lua
fruits = {"banana","orange","apple"}

-- 在末尾插入
table.insert(fruits,"mango")
print("索引为 4 的元素为 ",fruits[4])

-- 在索引为 2 的键处插入
table.insert(fruits,2,"grapes")
print("索引为 2 的元素为 ",fruits[2])

print("最后一个元素为 ",fruits[5])
table.remove(fruits)
print("移除后最后一个元素为 ",fruits[5])
```

执行以上代码输出结果为：

```
索引为 4 的元素为  mango
索引为 2 的元素为   grapes
最后一个元素为     mango
移除后最后一个元素为   nil
```

Table 排序

以下实例演示了 sort() 方法的使用，用于对 Table 进行排序：

```lua
fruits = {"banana","orange","apple","grapes"}
print("排序前")
for k,v in ipairs(fruits) do
  print(k,v)
end

table.sort(fruits)
print("排序后")
for k,v in ipairs(fruits) do
   print(k,v)
end
```

执行以上代码输出结果为：

```
排序前
1   banana
2   orange
3   apple
4    grapes
排序后
1  apple
2    banana
3   grapes
4   orange
```

Table 最大值

table.maxn 在 Lua5.2 之后该方法已经不存在了，我们定义了 table_maxn 方法来实现。

以下实例演示了如何获取 table 中的最大值：

```lua
function table_maxn(t)
    local mn = 0
    for k, v in pairs(t) do
        if mn < k then
            mn = k
        end
    end
    return mn
end
tbl = {[1] = "a", [2] = "b", [3] = "c", [26] = "z"}
print("tbl 长度 ", #tbl)
print("tbl 最大值 ", table_maxn(tbl))
```

执行以上代码输出结果为：

```
tbl 长度    3
tbl 最大值  26
```

## Lua 模块与包

模块类似于一个封装库，从 Lua 5.1 开始，Lua 加入了标准的模块管理机制，可以把一些公用的代码放在一个文件里，以 API 接口的形式在其他地方调用，有利于代码的重用和降低代码耦合度。

Lua 的模块是由变量、函数等已知元素组成的 table，因此创建一个模块很简单，就是创建一个 table，然后把需要导出的常量、函数放入其中，最后返回这个 table 就行。以下为创建自定义模块 module.lua，文件代码格式如下：

```lua
-- 文件名为 module.lua
-- 定义一个名为 module 的模块
module = {}
 
-- 定义一个常量
module.constant = "这是一个常量"
 
-- 定义一个函数
function module.func1()
    io.write("这是一个公有函数！\n")
end
 
local function func2()
    print("这是一个私有函数！")
end
 
function module.func3()
    func2()
end
 
return module
```

由上可知，模块的结构就是一个 table 的结构，因此可以像操作调用 table 里的元素那样来操作调用模块里的常量或函数。

上面的 func2 声明为程序块的局部变量，即表示一个私有函数，因此是不能从外部访问模块里的这个私有函数，必须通过模块里的公有函数来调用.
require 函数

Lua提供了一个名为require的函数用来加载模块。要加载一个模块，只需要简单地调用就可以了。例如：

```lua
require("<模块名>")
```

或者

```lua
require "<模块名>"
```

执行 require 后会返回一个由模块常量或函数组成的 table，并且还会定义一个包含该 table 的全局变量。

```lua
-- test_module.php 文件
-- module 模块为上文提到到 module.lua
require("module")
 
print(module.constant)
 
module.func3()
```

以上代码执行结果为：

```
这是一个常量
这是一个私有函数！
```

或者给加载的模块定义一个别名变量，方便调用：

```lua
-- test_module2.php 文件
-- module 模块为上文提到到 module.lua
-- 别名变量 m
local m = require("module")
 
print(m.constant)
 
m.func3()
```

以上代码执行结果为：

```
这是一个常量
这是一个私有函数！
```

### 加载机制

对于自定义的模块，模块文件不是放在哪个文件目录都行，函数 require 有它自己的文件路径加载策略，它会尝试从 Lua 文件或 C 程序库中加载模块。

require 用于搜索 Lua 文件的路径是存放在全局变量 package.path 中，当 Lua 启动后，会以环境变量 LUA_PATH 的值来初始这个环境变量。如果没有找到该环境变量，则使用一个编译时定义的默认路径来初始化。

当然，如果没有 LUA_PATH 这个环境变量，也可以自定义设置，在当前用户根目录下打开 .profile 文件（没有则创建，打开 .bashrc 文件也可以），例如把 "~/lua/" 路径加入 LUA_PATH 环境变量里：

```
#LUA_PATH
export LUA_PATH="~/lua/?.lua;;"
```

文件路径以 ";" 号分隔，最后的 2 个 ";;" 表示新加的路径后面加上原来的默认路径。

接着，更新环境变量参数，使之立即生效。

```
source ~/.profile
```

这时假设 package.path 的值是：

```
/Users/dengjoe/lua/?.lua;./?.lua;/usr/local/share/lua/5.1/?.lua;/usr/local/share/lua/5.1/?/init.lua;/usr/local/lib/lua/5.1/?.lua;/usr/local/lib/lua/5.1/?/init.lua
```

那么调用 require("module") 时就会尝试打开以下文件目录去搜索目标。

```
/Users/dengjoe/lua/module.lua;
./module.lua
/usr/local/share/lua/5.1/module.lua
/usr/local/share/lua/5.1/module/init.lua
/usr/local/lib/lua/5.1/module.lua
/usr/local/lib/lua/5.1/module/init.lua
```

如果找过目标文件，则会调用 package.loadfile 来加载模块。否则，就会去找 C 程序库。

搜索的文件路径是从全局变量 package.cpath 获取，而这个变量则是通过环境变量 LUA_CPATH 来初始。

搜索的策略跟上面的一样，只不过现在换成搜索的是 so 或 dll 类型的文件。如果找得到，那么 require 就会通过 package.loadlib 来加载它。
C 包

Lua和C是很容易结合的，使用C为Lua写包。

与Lua中写包不同，C包在使用以前必须首先加载并连接，在大多数系统中最容易的实现方式是通过动态连接库机制。

Lua在一个叫loadlib的函数内提供了所有的动态连接的功能。这个函数有两个参数:库的绝对路径和初始化函数。所以典型的调用的例子如下:

```
local path = "/usr/local/lua/lib/libluasocket.so"
local f = loadlib(path, "luaopen_socket")
```

loadlib函数加载指定的库并且连接到Lua，然而它并不打开库（也就是说没有调用初始化函数），反之他返回初始化函数作为Lua的一个函数，这样我们就可以直接在Lua中调用他。

如果加载动态库或者查找初始化函数时出错，loadlib将返回nil和错误信息。我们可以修改前面一段代码，使其检测错误然后调用初始化函数：

```lua
local path = "/usr/local/lua/lib/libluasocket.so"
-- 或者 path = "C:\\windows\\luasocket.dll"，这是 Window 平台下
local f = assert(loadlib(path, "luaopen_socket"))
f()  -- 真正打开库
```

一般情况下我们期望二进制的发布库包含一个与前面代码段相似的stub文件，安装二进制库的时候可以随便放在某个目录，只需要修改stub文件对应二进制库的实际路径即可。

将stub文件所在的目录加入到LUA_PATH，这样设定后就可以使用require函数加载C库了。 
