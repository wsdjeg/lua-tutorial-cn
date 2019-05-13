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
- [函数](#函数)
  - [可变参数](#可变参数)

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

````lua
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

在布尔表达式为 true 时会if中的代码块会被执行，在布尔表达式为 false 时，else 的代码块会被执行。

Lua认为false和nil为假，true 和非nil为真。要注意的是Lua中 0 为 true。

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


###  else if 语句

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

## 函数

函数定义语法：

```txt
optional_function_scope function function_name( argument1, argument2, argument3..., argumentn)
    function_body
    return result_params_comma_separated
end
````

解析：

- `optional_function_scope`: 该参数是可选的制定函数是全局函数还是局部函数，未设置该参数默认为全局函数，如果你需要设置函数为局部函数需要使用关键字 local。
- `function_name`: 指定函数名称。
- `argument1, argument2, argument3..., argumentn`: 函数参数，多个参数以逗号隔开，函数也可以不带参数。
- `function_body`: 函数体，函数中需要执行的代码语句块。
- `result_params_comma_separated`: 函数返回值，Lua 语言函数可以返回多个值，每个值以逗号隔开。

### 可变参数

Lua 函数可以接受可变数目的参数，和 C 语言类似，在函数参数列表中使用三点 ... 表示函数有可变的参数。

示例 1：

```lua
function add(...)
local s = 0
  for i, v in ipairs{...} do   --> {...} 表示一个由所有变长参数构成的数组
    s = s + v
  end
  return s
end
print(add(3,4,5,6,7))  --->25
```
