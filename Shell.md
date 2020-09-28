# Shell

# Shell脚本

Shell 脚本（shell script），是一种为 shell 编写的脚本程序。

业界所说的 shell 通常都是指 shell 脚本，但读者朋友要知道，shell 和 shell script 是两个不同的概念。

由于习惯的原因，简洁起见，本文出现的 "shell编程" 都是指 shell 脚本编程，不是指开发 shell 自身。

# Shell环境

Shell 编程跟 JavaScript、php 编程一样，只要有一个能编写代码的文本编辑器和一个能解释执行的脚本解释器就可以了。

Linux 的 Shell 种类众多，常见的有：

- Bourne Shell（/usr/bin/sh或/bin/sh）
- Bourne Again Shell（/bin/bash）
- C Shell（/usr/bin/csh）
- K Shell（/usr/bin/ksh）
- Shell for Root（/sbin/sh）
- ……

本教程关注的是 Bash，也就是 Bourne Again Shell，由于易用和免费，Bash 在日常工作中被广泛使用。同时，Bash 也是大多数Linux 系统默认的 Shell。

在一般情况下，人们并不区分 Bourne Shell 和 Bourne Again Shell，所以，像 **#!/bin/sh**，它同样也可以改为 **#!/bin/bash**。

\#! 告诉系统其后路径所指定的程序即是解释此脚本文件的 Shell 程序。

# 第一个Shell脚本

打开文本编辑器(可以使用 vi/vim 命令来创建文件)，新建一个文件 test.sh，扩展名为 sh（sh代表shell），扩展名并不影响脚本执行，见名知意就好，如果你用 php 写 shell 脚本，扩展名就用 php 好了。

输入一些代码，第一行一般是这样：

```shell
#!/bin/bash
echo "Hello World !"
```

\#! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。

echo 命令用于向窗口输出文本。

## 运行 Shell 脚本有两种方法：

**1、作为可执行程序**

将上面的代码保存为 test.sh，并 cd 到相应目录：

```shell
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```

注意，一定要写成 ./test.sh，而不是 **test.sh**，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin,  /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用  ./test.sh 告诉系统说，就在当前目录找。

**2、作为解释器参数**

这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如：

```shell
/bin/sh test.sh
/bin/php test.php
```

这种方式运行的脚本，不需要在第一行指定解释器信息，写了也没用。

# Shell变量

定义变量时，变量不用加美元符号，如：

```shell
your_name="xxx"
```

注意，变量名和等号之间不能有空格，这可能和你熟悉的所有编程语言都不一样。同时，变量名的命名须遵循如下规则：

- ​		命名只能使用英文字母，数字和下划线，首个字符不能以数字开头。
- ​		中间不能有空格，可以使用下划线（_）。
- ​		不能使用标点符号。
- ​		不能使用bash里的关键字（可用help命令查看保留关键字）。

有效的 Shell 变量名示例如下：

```shell
RUNOOB
LD_LIBRARY_PATH
_var
var2
```

无效的变量命名：

```shell
?var=123
user*name=runoob
1ze=123
```

除了显式地直接赋值，还可以用语句给变量赋值，如：

```shell
for file in `ls /etc`
或
for file in $(ls /etc)
```

以上语句将 /etc 下目录的文件名循环出来。

## 使用变量

使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

```
your_name="qinjx"
echo $your_name
echo ${your_name}
```

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况：

```
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
```

如果不给skill变量加花括号，写成echo "I am good at $skillScript"，解释器就会把$skillScript当成一个变量（其值为空），代码执行结果就不是我们期望的样子了。

推荐给所有变量加上花括号，这是个好的编程习惯。

已定义的变量，可以被重新定义，如：

```
your_name="tom"
echo $your_name
your_name="alibaba"
echo $your_name
```

这样写是合法的，但注意，第二次赋值的时候不能写$your_name="alibaba"，使用变量的时候才加美元符（$）。

## 只读变量

使用 readonly 命令可以将变量定义为只读变量，只读变量的值不能被改变。

下面的例子尝试更改只读变量，结果报错：

```
#!/bin/bash
myUrl="https://www.google.com"
readonly myUrl
myUrl="https://www.runoob.com"
```

运行脚本，结果如下：

```
/bin/sh: NAME: This variable is read only.
```

## 删除变量

使用 unset 命令可以删除变量。语法：

```
unset variable_name
```

变量被删除后不能再次使用。unset 命令不能删除只读变量。

**实例**

```
#!/bin/sh
myUrl="https://www.runoob.com"
unset myUrl
echo $myUrl
```

以上实例执行将没有任何输出。

## 变量类型

运行shell时，会同时存在三种变量：

- **1) 局部变量** 局部变量在脚本或命令中定义，仅在当前shell实例中有效，其他shell启动的程序不能访问局部变量。
- **2) 环境变量** 所有的程序，包括shell启动的程序，都能访问环境变量，有些程序需要环境变量来保证其正常运行。必要的时候shell脚本也可以定义环境变量。
- **3) shell变量** shell变量是由shell程序设置的特殊变量。shell变量中有一部分是环境变量，有一部分是局部变量，这些变量保证了shell的正常运行

------

## Shell 字符串

字符串是shell编程中最常用最有用的数据类型（除了数字和字符串，也没啥其它类型好用了），字符串可以用单引号，也可以用双引号，也可以不用引号。单双引号的区别跟PHP类似。

### 单引号

```
str='this is a string'
```

单引号字符串的限制：

- ​		单引号里的任何字符都会原样输出，单引号字符串中的变量是无效的；
- ​		单引号字串中不能出现单独一个的单引号（对单引号使用转义符后也不行），但可成对出现，作为字符串拼接使用。

### 双引号

```
your_name='runoob'
str="Hello, I know you are \"$your_name\"! \n"
echo -e $str
```

输出结果为：

```
Hello, I know you are "runoob"! 
```

双引号的优点：

- ​		双引号里可以有变量
- ​		双引号里可以出现转义字符

### 拼接字符串

```
your_name="runoob"
# 使用双引号拼接
greeting="hello, "$your_name" !"
greeting_1="hello, ${your_name} !"
echo $greeting  $greeting_1
# 使用单引号拼接
greeting_2='hello, '$your_name' !'
greeting_3='hello, ${your_name} !'
echo $greeting_2  $greeting_3
```

输出结果为：

```
hello, runoob ! hello, runoob !
hello, runoob ! hello, ${your_name} !
```

### 获取字符串长度

```
string="abcd"
echo ${#string} #输出 4
```

### 提取子字符串

以下实例从字符串第 **2** 个字符开始截取 **4** 个字符：

```
string="runoob is a great site"
echo ${string:1:4} # 输出 unoo
```

**注意**：第一个字符的索引值为 **0**。

### 查找子字符串

查找字符 **i** 或 **o** 的位置(哪个字母先出现就计算哪个)：

```
string="runoob is a great site"
echo `expr index "$string" io`  # 输出 4
```

**注意：** 以上脚本中 ` 是反引号，而不是单引号 '，不要看错了哦。

------

## Shell 数组

bash支持一维数组（不支持多维数组），并且没有限定数组的大小。

类似于 C 语言，数组元素的下标由 0 开始编号。获取数组中的元素要利用下标，下标可以是整数或算术表达式，其值应大于或等于 0。

### 定义数组

在 Shell 中，用括号来表示数组，数组元素用"空格"符号分割开。定义数组的一般形式为：

```
数组名=(值1 值2 ... 值n)
```

例如：

```
array_name=(value0 value1 value2 value3)
```

或者

```
array_name=(
value0
value1
value2
value3
)
```

还可以单独定义数组的各个分量：

```
array_name[0]=value0
array_name[1]=value1
array_name[n]=valuen
```

可以不使用连续的下标，而且下标的范围没有限制。

### 读取数组

读取数组元素值的一般格式是：

```
${数组名[下标]}
```

例如：

```
valuen=${array_name[n]}
```

使用 @ 符号可以获取数组中的所有元素，例如：

```
echo ${array_name[@]}
```

### 获取数组的长度

获取数组长度的方法与获取字符串长度的方法相同，例如：

```shell
# 取得数组元素的个数
length=${#array_name[@]}
# 或者
length=${#array_name[*]}
# 取得数组单个元素的长度
lengthn=${#array_name[n]}
```

------

## Shell 注释

以 # 开头的行就是注释，会被解释器忽略。

通过每一行加一个 **#** 号设置多行注释，像这样：

```shell
#--------------------------------------------
# 这是一个注释
# author：菜鸟教程
# site：www.runoob.com
# slogan：学的不仅是技术，更是梦想！
#--------------------------------------------
##### 用户配置区 开始 #####
#
#
# 这里可以添加脚本描述信息
# 
#
##### 用户配置区 结束  #####
```

如果在开发过程中，遇到大段的代码需要临时注释起来，过一会儿又取消注释，怎么办呢？

每一行加个#符号太费力了，可以把这一段要注释的代码用一对花括号括起来，定义成一个函数，没有地方调用这个函数，这块代码就不会执行，达到了和注释一样的效果。

### 多行注释

多行注释还可以使用以下格式：

```shell
:<<EOF
注释内容...
注释内容...
注释内容...
EOF
```

EOF 也可以使用其他符号:

```shell
:<<'
注释内容...
注释内容...
注释内容...
'

:<<!
注释内容...
注释内容...
注释内容...
!
```

# Shel传递参数

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：**$n**。**n** 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

## 实例

以下实例我们向脚本传递三个参数，并分别输出，其中 **$0** 为执行的文件名（包含文件路径）：

```shell
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

为脚本设置可执行权限，并执行脚本，输出结果如下所示：

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
执行的文件名：./test.sh
第一个参数为：1
第二个参数为：2
第三个参数为：3
```

| 参数处理 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| $#       | 获取传递到脚本的参数个数，不包含$0                           |
| $*       | 以一个单字符串的形式显式所有向脚本传输的参数。               |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程ID号                                   |
| $@       | 与$*同样作用，但形式是每个参数为一个字符串。                 |
| $-       | 显示Shell使用的当前选项，与set命令功能相同                   |
| $?       | 显示最后一个命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

```shell
#!/bin/bash

echo "Shell参数传递举例";
echo "第一个参数为：$1";

echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```

执行脚本，输出结果如下所示：

```shell
$ chmod +x test.sh 
$ ./test.sh 1 2 3
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3
```

## $* 与 $@

- 相同点：作用相同，都是引用所有参数
- 不同点：**只有在双引号中体现出来。**假设在脚本运行时写了三个参数 1、2、3，，则 " * " 等价于 "1 2 3"（传递了一个参数），而 "@" 等价于 "1" "2" "3"（传递了三个参数）。

```shell
#!/bin/bash

echo "-- \$* 演示 ---"
for i in "$*"; do
    echo $i ##只执行了一次，因为只有一个字符串
done

echo "-- \$@ 演示 ---"
for i in "$@"; do
    echo $i ##执行多次，因为有多个字符串
done
```

执行脚本，输出结果如下所示：

```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
-- $* 演示 ---
1 2 3
-- $@ 演示 ---
1
2
3
```

​	PS1：在为shell脚本传递的参数中**如果包含空格，应该使用单引号或者双引号将该参数括起来，以便于脚本将这个参数作为整体来接收**。

​	PS2：在有参数时，可以使用对参数进行校验的方式处理以减少错误发生：

```shell
if [ -n "$1" ]; then
    echo "包含第一个参数"
else
    echo "没有包含第一参数"
fi
```

**注意**：中括号 [] 与其中间的代码应该有空格隔开

# Shell数组

数组中可以存放多个值。**Bash Shell 只支持一维数组（不支持多维数组），初始化时不需要定义数组大小**（与 PHP 类似）。

与大部分编程语言类似，数组元素的下标由0开始。

Shell 数组用括号来表示，**元素用"空格"符号分割开**，语法格式如下：

```shell
array_name=(value1 value2 ... valuen)
```

## 实例

```shell
#!/bin/bash

a="a"
arr=(A "B" 'C' ${a}) ## 赋值语句，可以将变量赋值给数组元素

echo "数组第1个元素："${arr[0]} ## 单个输出
echo "数组第2个元素："${arr[1]}
echo "数组第3个元素："${arr[2]}
echo "数组第4个元素："${arr[3]}
echo "数组所有元素："${arr[@]} ## 打印数组中所有的元素
echo "数组所有元素："${arr[*]} ## 同上
echo "数组的长度："${#arr[@]} ## 获取数组的长度，同字符串

b="b"
arr[4]=${b} ## 也可以使用下标定义数组
echo "数组第5个元素："${arr[4]}
echo "数组所有元素："${arr[@]}
echo "数组的长度："${#arr[@]}
```

执行脚本，输出结果如下：

```shell
数组第1个元素：A
数组第2个元素：B
数组第3个元素：C
数组第4个元素：a
数组所有元素：A B C a
数组所有元素：A B C a
数组的长度：4
数组第5个元素：b
数组所有元素：A B C a b
数组的长度与：5
```

# Shell运算符

Shell 和其他编程语言一样，支持多种运算，包括：

- 算术运算符
- 关系运算符
- 布尔运算符
- 逻辑运算符
- 字符串运算符
- 文件测试运算符

原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。

expr 是一款表达式计算工具，使用它能完成表达式的求值操作。

例如，两个数相加(**注意使用的是反引号 ` 而不是单引号 '**)：

```shell
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为： ${val}" ##输出4
```

需要注意的是：

- 表达式和运算符之间要有空格，例如2+2是不对的（每空格），必须写成 2 + 2 （有空格）。
- 完整的表达式要被**``**（两个反引号）包含。

## 算术运算符

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                    | 举例                         |
| ------ | ------------------------------------------------------- | ---------------------------- |
| +      | 加法                                                    | \`expr $a + $b\`结果为30     |
| -      | 减法                                                    | \`expr $a - $b\` 结果为-10   |
| *      | 乘法                                                    | \`expr $a \\* $b\` 结果为200 |
| /      | 除法                                                    | \`expr $b / $a\` 结果为2     |
| %      | 求余                                                    | \`expr $b % $a\` 结果为0     |
| =      | 赋值                                                    | a=$b ，将变量b的值赋值给a    |
| ==     | 相等。比较左右两个值，相同则返回true，反之返回false     | [ $a == $b ] 返回false       |
| !=     | 不相等。比较左右两个值，不相同则返回true，反之返回false | [ $a != $b ] 返回true        |

**注意：条件表达式要放在方括[ ]中，并且要有空格，例如：[$a!=$b]是错误的**

### 实例

```shell
#!/bin/bash

a=10
b=20

val=`expr $a + $b`
echo "a + b: ${val}"

val=`expr $a - $b`
echo "a - b: ${val}"

val=`expr $a \* $b`
echo "a * b: ${val}"

val=`expr $b / $a`
echo "b / a: ${val}"

val=`expr $b % $a`
echo "b % a: ${val}"

if [ $a == $b ] 
then
	echo "a 等于 b"
fi

if [ $a != $b ]
then 
	echo "a 不等于 b"
fi
```

执行结果如下：

```txt
a + b: 30
a - b: -10
a * b: 200
b / a: 2
b % a: 0
a 不等于 b
```

**注意：**

- **乘号（*）前边必须加上转义字符（\）才能实现乘法运算**
- **if...then...fi 是条件语句**
- **在MAC中的shell的expr语法是 $((表达式))，此处表达式乘法不需要转义字符**

## 关系运算符

**关系运算符只支持数字，不支持字符串，除非字符串的值是数字。**

下表列出了常用的关系运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                             | 举例                    |
| ------ | ------------------------------------------------ | ----------------------- |
| -eq    | 检测两个数是否相等，相等返回true                 | [ $a -eq $b ] 返回false |
| -ne    | 检测两个数是否不相等，不相等返回true             | [ $a -ne $b ] 返回true  |
| -gt    | 检测左边的数是否大于右边的数，如果大于返回true   | [ $a -gt $b ] 返回false |
| -lt    | 检测左边的数是否小于右边的数，如果小于返回true   | [ $a -lt $b ] 返回true  |
| -ge    | 检测左边的数是否大于等于右边的数，如果是返回true | [ $a -ge $b ] 返回false |
| -le    | 检测左边的数是否小于等于右边的数，如果是返回true | [ $a -le $ b ] 返回true |

**英语记忆法：e=equal、n=never、g=greater、l=less、t=than ** 

### 实例

```shell
#!/bin/bash

a=10
b=20

if [ $a -eq $b ]
then
	echo "$a -eq $b : a 等于 b"
else
	echo "$a -eq $b : a 不等于 b"
fi
if [ $a -ne $b ]
then
	echo "$a -ne $b : a 不等于 b"
else 
	echo "$a -ne $b : a 等于 b"
fi
if [ $a -gt $b ]
then
	echo "$a -gt $b : a 大于 b"
else
	echo "$a -gt $b : a 不大于 b"
fi
if [ $a -lt $b ]
then 
	echo "$a -lt $b : a 小于 b"
else
	echo "$a -lt $b : a 不小于 b"
fi
if [ $a -ge $b ]
then
	echo "$a -ge $b : a 大于或等于 b"
else
	echo "$a -ge $b : a 小于 b"
fi
if [ $a -le $b ]
then
	echo "$a -le $b : a 小于或等于 b"
else
	echo "$a -ge $b : a 大于 b"
fi
```

执行脚本，结果如下：

```
10 -eq 20 : a 不等于 b
10 -ne 20 : a 不等于 b
10 -gt 20 : a 不大于 b
10 -lt 20 : a 小于 b
10 -ge 20 : a 小于 b
10 -le 20 : a 小于或等于 b
```

## 布尔运算符

下表列出了常用的布尔运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                                 | 举例                                 |
| ------ | ---------------------------------------------------- | ------------------------------------ |
| ！     | 非运算，表达式为true则返回false，否则返回true        | [ ! false ] 返回true                 |
| -o     | 或运算，左右表达式都为false则返回false，否则返回true | [ $a -lt $b -o $a -gt $b ] 返回true  |
| -a     | 与运算，左右表达式都为true则返回true，否则返回false  | [ $a -lt $b -o $a -gt $b ] 返回false |

### 实例

```shell
#!/bin/bash

a=10
b=20

if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a == $b: a 等于 b"
fi
if [ $a -lt 100 -a $b -gt 15 ]
then
   echo "$a 小于 100 且 $b 大于 15 : 返回 true"
else
   echo "$a 小于 100 且 $b 大于 15 : 返回 false"
fi
if [ $a -lt 100 -o $b -gt 100 ]
then
   echo "$a 小于 100 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 100 或 $b 大于 100 : 返回 false"
fi
if [ $a -lt 5 -o $b -gt 100 ]
then
   echo "$a 小于 5 或 $b 大于 100 : 返回 true"
else
   echo "$a 小于 5 或 $b 大于 100 : 返回 false"
fi
```

执行脚本，结果如下：

```shell
10 != 20 : a 不等于 b
10 小于 100 且 20 大于 15 : 返回 true
10 小于 100 或 20 大于 100 : 返回 true
10 小于 5 或 20 大于 100 : 返回 false
```

## 逻辑运算符

以下介绍 Shell 的逻辑运算符，假定变量 a 为 10，变量 b 为 20:

| 运算符 | 说明   | 举例                                       |
| ------ | ------ | ------------------------------------------ |
| &&     | 逻辑与 | [[ $a -lt 100 && $b -gt 100 ]] 返回 false  |
| \|\|   | 逻辑或 | [[ $a -lt 100 \|\| $b -gt 100 ]] 返回 true |

### 实例

```shell
#!/bin/bash

a=10
b=20
 
if [[ $a -lt 100 && $b -gt 100 ]]
then
  echo "返回 true"
else
  echo "返回 false"
fi
 
if [[ $a -lt 100 || $b -gt 100 ]]
then
   echo "返回 true"
else
   echo "返回 false"
fi
```

执行脚本，输出结果如下所示：

```shell
返回 false
返回 true
```

**注意：条件表达式内还有一层中括号[]，如果不加则会发生错误：**

```shell
#!/bin/bash

a=10
b=20

if [ $a -lt $b && $b -lt 100 ]
then
        echo "$a 小于 $b 且 $b 小于 100 ： 返回true"
else
        echo "$a 小于 $b 且 $b 小于 100 ： 返回false"
fi
```

运行脚本，结果（错误）如下：

```
./test.sh: line 6: [: missing `]'
10 小于 20 且 20 小于 100 ： 返回false 
```

## 字符串运算符

下表列出了常用的字符串运算符，假定变量 a 为 "abc"，变量 b 为 "efg"：

| 运算符 | 说明                                   | 举例                   |
| ------ | -------------------------------------- | ---------------------- |
| =      | 检测两个字符串是否相等，相等返回true   | [ $a = $b ] 返回 false |
| !=     | 检测两个字符串是否相等，不相等返回true | [ $a != $b ] 返回true  |
| -z     | 检测字符串长度是否为0，为0返回true     | [ -z $a ] 返回false    |
| -n     | 检测字符串长度是否不为0，不为0返回true | [ -n $b ] 返回true     |
| $      | 检测字符串是否为空，不为空返回true     | [ $a ] 返回true        |

### 实例

```shell
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ]
then
	echo "$a = $b : a 等于 b"
else
	echo "$a = $b : a 不等于 b"
fi
if [ $a != $b ]
then
	echo "$a != $b : a 不等于 b"
else
	echo "$a != $b : a 等于 b"
fi
if [ -z $a ]
then
	echo "-z $a : 字符串a长度为0"
else 
	echo "-z $a : 字符串a长度不为0"
fi
if [ -n $a ]
then 
	echo "-n $a : 字符串a长度不为0"
else
	echo "-n $a : 字符串a长度为0"
fi
if [ $a ]
then
	echo "$a : 字符串a不为空"
else
	echo "$a : 字符串a为空"
fi
```

执行脚本，结果如下：

```
abc = efg : a 不等于 b
abc != efg : a 不等于 b
-z abc : 字符串a长度不为0
-n abc : 字符串a长度不为0
abc : 字符串a不为空
```

## 文件测试运算符

文件测试运算符用于检测 Unix 文件的各种属性。

属性检测描述如下：

| 操作符  | 说明                                                         |
| ------- | ------------------------------------------------------------ |
| -b file | 检测文件是否是块设备文件，如果是，则返回true                 |
| -c file | 检测文件是否是字符设备文件，如果是，则返回true               |
| -d file | 检测文件是否是目录，如果是，则返回true                       |
| -f file | 检测文件是否为普通文件(不是目录，不是设备文件)，如果是，返回true |
| -g file | 检测文件是否设置了SGID位，如果是，返回true                   |
| -u file | 检测文件是否设置了SUID位，如果是，返回true                   |
| -k file | 检测文件是否设置了粘着位（Sticky Bit），如果是，则返回true   |
| -p file | 检测文件是否是有名（命名）管道，如果是，则返回true           |
| -S file | 检测文件是否是 socket文件，如果是，则返回true                |
| -L file | 检测文件是否存在并且是一个符号连接，如果是，则返回true       |
| -r file | 检测文件是否可读，如果是，则返回true                         |
| -w file | 检测文件是否可写，如果是，则返回true                         |
| -x file | 检测文件是否可执行，如果是，则返回true                       |
| -s file | 检测文件是否为空（文件大小是否大于0），不为空返回true        |
| -e file | 检测文件（包括目录）是否存在，如果是，则返回true             |

### 实例

变量 file 表示文件 **/var/www/runoob/test.sh**，它的大小为 100 字节，具有 **rwx** 权限。下面的代码，将检测该文件的各种属性：

```shell
#!/bin/bash

file="/var/www/runoob/test.sh"
if [ -r $file ]
then
   echo "文件可读"
else
   echo "文件不可读"
fi
if [ -w $file ]
then
   echo "文件可写"
else
   echo "文件不可写"
fi
if [ -x $file ]
then
   echo "文件可执行"
else
   echo "文件不可执行"
fi
if [ -f $file ]
then
   echo "文件为普通文件"
else
   echo "文件为特殊文件"
fi
if [ -d $file ]
then
   echo "文件是个目录"
else
   echo "文件不是个目录"
fi
if [ -s $file ]
then
   echo "文件不为空"
else
   echo "文件为空"
fi
if [ -e $file ]
then
   echo "文件存在"
else
   echo "文件不存在"
fi
```

执行脚本，输出结果如下所示：

```shell
文件可读
文件可写
文件可执行
文件为普通文件
文件不是个目录
文件不为空
文件存在
```

### SUID和SGID

- SUID 是 Set User ID
- SGID 是 Set Group ID

​	SUID和SGID是在执行程序（程序的可执行位被设置）时起作用，而可执行位只对普通文件和目录文件有意义，所以设置其他种类文件的SUID和SGID位是没有多大意义的。

#### 设置

如果一个文件被设置了SUID或SGID位，会分别表现在所有者或同组用户的权限的可执行位上。例如：

1、**-rwsr-xr-x** 表示SUID和所有者权限中可执行位被设置

2、**-rwSr--r--** 表示SUID被设置，但所有者权限中可执行位没有被设置

3、**-rwxr-sr-x** 表示SGID和同组用户权限中可执行位被设置

4、**-rw-r-Sr--** 表示SGID被设置，但同组用户权限中可执行位没有被设置

**PS：特殊标志位（SUID、SGID、sticky）会占用 x 标志位的位置，所以用大小区分原本的权限是否设置了 x。小写表示原本设置了 x；大小表示原本没有设置**

#### 详细解析

​	首先讲普通文件的SUID和SGID的作用。例子：

​	如果普通文件myfile是属于foo用户的，是可执行的，现在没设SUID位，ls命令显示如下：

​	**-rwxr-xr-x 1 foo staff 7734 Apr 05 17:07  myfile** 任何用户都可以执行这个程序。**UNIX的内核是根据什么来确定一个进程对资源的访问权限的呢？是这个进程的运行用户的（有效）ID，包括user id和group id。**用户可以用id命令来查到自己的或其他用户的user id和group id。

​	除了一般的user id 和group id外，还有两个称之为effective 的id，就是有效id，上面的四个id表示为：uid，gid，euid，egid。**内核主要是根据euid和egid来确定进程对资源的访问权限。**

​	**一个进程如果没有SUID或SGID位，则euid=uid  egid=gid，分别是运行这个程序的用户的uid和gid。**例如kevin用户的uid和gid分别为204和202，foo用户的uid和gid为200，201，kevin运行myfile程序形成的进程的euid=uid=204，egid=gid=202，内核根据这些值来判断进程对资源访问的限制，其实就是kevin用户对资源访问的权限，和foo没关系。

​	**如果一个程序设置了SUID，则euid和egid变成被运行的程序的所有者的uid和gid，**例如kevin用户运行myfile，euid=200，egid=201，uid=204，gid=202，则这个进程具有它的属主foo的资源访问权限。

​	**SUID的作用就是这样：让本来没有相应权限的用户运行这个程序时，可以访问他没有权限访问的资源。**passwd就是一个很鲜明的例子。

​	**SUID的优先级比SGID高，当一个可执行程序设置了SUID，则SGID会自动变成相应的egid。**

### 粘着位（sticky bit）

​	如果用户对目录有写权限，则可以删除该目录下的文件和子目录，即使该用户不是这些文件的所有者，而且也没有读或写许可。**粘着位出现执行许可的位置上，用t表示**。设置了该位后，其它用户就不可以删除不属于他的文件和目录。但是该目录下的目录不继承该权限，要再设置才可使用。

​	如果没有粘着位，因为普通用户有w权限，所以可以删除此目录下所有文件，包括其他用户建立的文件。一旦赋予了粘着位，除了root可以删除所有文件外，普通用户就算拥有w权限，也只能删除自己建立的文件，但是不能删除其他用户建立的文件。

​	**普通文件的sticky位会被linux内核忽略。**

​	**目录的sticky位表示这个目录里的文件只能被owner和root删除 。**



# Shell echo命令

Shell 的 echo 指令与 PHP 的 echo 指令类似，都是用于字符串的输出。命令格式：

```shell
echo string
```

您可以使用echo实现更复杂的输出格式控制。

## 1.显示普通字符串:

```shell
echo "It is a test"
```

这里的双引号完全可以省略，以下命令与上面实例效果一致：

```shell
echo It is a test
```

## 2.显示转义字符

```shell
echo "\"It is a test\""
```

结果将是:

```
"It is a test"
```

同样，双引号也可以省略

```shell
echo \"It is a test\"
```

## 3.显示变量

**read 命令从标准输入中读取一行,并把输入行的每个字段的值指定给 shell 变量**

```shell
#!/bin/sh
read name 
echo "$name It is a test"
```

以上代码保存为 test.sh，name 接收标准输入的变量，结果将是: 

```
[root@www ~]# sh test.sh
OK                     #标准输入
OK It is a test        #输出
```

## 4.显示换行

**-e：开启转义**

```shell
echo -e "OK! \n" # -e 开启转义
echo "It is a test"
```

输出结果：

```
OK!

It is a test
```

## 5.显示不换行

**\c：不换行**

```shell
#!/bin/sh
echo -e "OK! \c" # -e 开启转义 \c 不换行
echo "It is a test"
```

输出结果：

```shell
OK! It is a test
```

## 6.显示结果定向至文件

```shell
echo "It is a test" > myfile
```

## 7.原样输出字符串，不进行转义或取变量(用单引号)

**单引号中的内容原样输出**

```shell
echo '$name\"'
```

输出结果：

```
$name\"
```

## 8.显示命令执行结果

```
echo `date`
```

**注意：** 这里使用的是反引号 `, 而不是单引号 '。

结果将显示当前日期

```shell
Thu Jul 24 10:08:46 CST 2014
```

# Shell printf命令

printf 命令模仿 C 程序库（library）里的 printf() 程序。

printf 由 POSIX 标准所定义，因此使用 printf 的脚本比使用 echo 移植性好。

printf 使用引用文本或空格分隔的参数，外面可以在 printf 中使用格式化字符串，还可以制定字符串的宽度、左右对齐方式等。默认 printf 不会像 echo 自动添加换行符，我们可以手动添加 \n。

printf 命令的语法：

```shell
printf  format-string  [arguments...]
```

**参数说明：**

- **format-string:** 为格式控制字符串
- **arguments:** 为参数列表。

## 实例

```shell
$ echo "hello,shell"
hello,shell
$ printf "hello,shell\n"
hello,shell
$
```

接下来,我来用一个脚本来体现printf的强大功能：

```shell
#!/bin/bash
 
printf "%-10s %-8s %-4s\n" 姓名 性别 体重kg  
printf "%-10s %-8s %-4.2f\n" 郭靖 男 66.1234 
printf "%-10s %-8s %-4.2f\n" 杨过 男 48.6543 
printf "%-10s %-8s %-4.2f\n" 郭芙 女 47.9876 
```

执行脚本，输出结果如下所示：

```
姓名     性别   体重kg
郭靖     男      66.12
杨过     男      48.65
郭芙     女      47.99
```

%s %c %d %f都是格式替代符

%-10s 指一个宽度为10个字符（-表示左对齐，没有则表示右对齐），任何字符都会被显示在10个字符宽的字符内，如果不足则自动以空格填充，超过也会将内容全部显示出来。

%-4.2f 指格式化为小数，其中.2指保留2位小数。

更多实例：

```shell
#!/bin/bash

# format-string为双引号
printf "%d %s\n" 1 "abc"

# 单引号与双引号效果一样 
printf '%d %s\n' 1 "abc" 

# 没有引号也可以输出
printf %s abcdef

# 格式只指定了一个参数，但多出的参数仍然会按照该格式输出，format-string 被重用
printf %s abc def

printf "%s\n" abc def

printf "%s %s %s\n" a b c d e f g h i j

# 如果没有 arguments，那么 %s 用NULL代替，%d 用 0 代替
printf "%s and %d \n" 
```

执行脚本，输出结果如下所示：

```shell
1 abc
1 abc
abcdefabcdefabc ## 这里是第10行 + 第13行　＋　第15行的abc 的打印结果
def
a b c
d e f
g h i
j  
 and 0
```

## printf的转义序列

| 序列  | 说明                                                         |
| ----- | ------------------------------------------------------------ |
| \a    | 警告字符，通常为ASCII的BEL字符                               |
| \b    | 后退                                                         |
| \c    | （抑制（不显示）输出结果中任何结尾的换行字符（只在%b格式指示符控制下的参数字符串中有效），而且，任何留在参数里的字符、任何接下来的参数以及任何留在格式字符串中的字符，都被忽略） |
| \f    | 换页（formfeed）                                             |
| \n    | 换行                                                         |
| \r    | 回车（Carriage return）                                      |
| \t    | 水平制表符                                                   |
| \v    | 垂直制表符                                                   |
| \\\   | 一个字符上的反斜杠字符                                       |
| \ddd  | 表示1到3位数八进制的字符。仅在格式字符串中有效               |
| \0ddd | 表示1到3位数八进制的字符                                     |

### 实例

```shell
$ printf "a string, no processing:<%s>\n" "A\nB"
a string, no processing:<A\nB>

$ printf "a string, no processing:<%b>\n" "A\nB"
a string, no processing:<A
B>

$ printf "www.runoob.com \a"
www.runoob.com $                  #不换行
```

# Shell test命令

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

## 数值测试

| 参数 | 说明           |
| ---- | -------------- |
| -eq  | 等于则为真     |
| -ne  | 不等于则为真   |
| -gt  | 大于则为真     |
| -lt  | 小于则为真     |
| -ge  | 大于等于则为真 |
| -le  | 小于等于则为真 |

### 实例

```shell
#!/bin/bash
num1=100
num2=200
if test $[num1] -eq $[num2]
then
	echo "两数相等"
else
	echo "两数不相等"
fi
```

输出结果为： **两数不相等**

代码中 **[ ] (中括号)** 执行基本的算术运算，如：

```shell
#!/bin/bash

a=5
b=6
result=$[a+b]
echo "result 为：$result"
```

结果为： **result为： 11**

## 字符串测试

| 参数      | 说明                     |
| --------- | ------------------------ |
| =         | 等于则为真               |
| !=        | 不相等则为真             |
| -z 字符串 | 字符串的长度为零则为真   |
| -n 字符串 | 字符串的长度不为零则为真 |

### 实例

```shell
#!/bin/bash
str1="aaaa"
str2="bbbb"
if test $[str1] = $[str2]
then 
	echo "两个字符串相等"
else
	echo "两个字符串不相等"
fi
```

结果为：**两个字符串不相等**

## 文件测试



| 参数      | 说明                               |
| --------- | ---------------------------------- |
| -e 文件名 | 如果文件存在为真                   |
| -r 文件名 | 如果文件存在且可读则为真           |
| -w 文件名 | 如果文件存在且可写则为真           |
| -x 文件名 | 如果文件存在且可执行则为真         |
| -s 文件名 | 如果文件存在且至少有一个字符则为真 |
| -d 文件名 | 如果文件存在且为目录则为真         |
| -f 文件名 | 如果文件存在且为普通文件则为真     |
| -c 文件名 | 如果文件存在且为字符文件则为真     |
| -b 文件名 | 如果文件存在且为块设备文件则为真   |

### 实例

```shell
#!/bin/bash
if test -e ./test.sh
then
	echo "文件已存在"
else
	echo "文件不存在"
fi
```

输出结果为：**文件已存在**

​	另外，Shell 还提供了与( -a )、或( -o )、非( ! )三个逻辑操作符用于将测试条件连接起来，其**优先级为： ! 最高， -a 次之， -o 最低。**例如：

### 实例

```shell
#!/bin/bash
cd /bin
if test -e ./notFile -o -e ./bash
then
    echo '至少有一个文件存在!'
else
    echo '两个文件都不存在'
fi
```

输出结果：**至少有一个文件存在!**

# Shell流程控制

和Java、PHP等语言不一样，sh的流程控制不可为空，如(以下为PHP流程控制写法)：

```php
<?php
if (isset($_GET["q"])) {
    search(q);
}
else {
    // 不做任何事情
}
```

在sh/bash里可不能这么写，如果else分支没有语句执行，就不要写这个else。

## if-else

### if

if 语句语法格式：

```shell
if condition
then
	command1
	command2
	...
	commandN
fi	
```

 可以写成一行（适用于终端命令提示符）:

```shell
if [ $(ps -ef | grep -c "ssh") -gt 1 ];then echo "true"; fi
```

末尾的fi就是if倒过来写的。

### if else

if else 语法格式：

```shell
if condition
then 
	command1
	...
	commandN
else 
	command1
	...
	commandN
fi
```

### if else-if else

语法格式：

```shell
if condition1
then
	command1
elif condition2
then
	command2
else
	commandN
fi
```

#### 实例

```shell
#!/bin/bash

a=10
b=20
if [ $a == $b ]	
then
	echo "a 等于 b"
elif [ $a -lt $b ]
then
	echo "a 小于 b"
elif [ $a -gt $b ]
then
	echo "a 大于 b"
else 
	echo "a	与 b 无法比较"
fi
```

输出结果：**a 小于 b**

if else语句经常与test命令结合使用。

## for循环

与其他编程语言类似，Shell支持for循环

一般格式为：

```shell
for var in item1 item2 ... itemN
do 
	command1
	command2
	...
	commandN
done
```

也可以写成一行：

```shell
for var in item1 item2 ... itemN; do command1 ... commandN; done
```

当变量值在列表里，for循环即执行一次所有命令，使用变量名获取列表中的当前取值。命令可为任何有效的shell命令和语句。in列表可以包含替换、字符串和文件名。

in列表是可选的，如果不用它，for循环使用命令行的位置参数。

**in 后面可以接数组和变量**

```shell
#!/bin/bash
arr=(1 2 3 4 5)
for loop in ${arr[@]} # 相当于 1 2 3 4 5
do
    echo "current loop is $loop"
done
```

输出结果：

```shell
current loop is 1
current loop is 2
current loop is 3
current loop is 4
current loop is 5
```

**不使用in的情况**

```shell
#!/bin/bash

for loop
do
    echo "current loop is $loop"
done

```

执行脚本 **./test.sh 1 2 3 4 5**，输出结果为：

```shell
current loop is 1
current loop is 2
current loop is 3
current loop is 4
current loop is 5
```

顺序输出字符串中的字符：

```shell
for str in 'This is a string'
do
    echo $str
done
```

输出结果：

```shell
This is a string
```

shell 中的 for 循环不仅可以用如上所述的方法。

对于习惯其他语言 for 循环的朋友来说可能有点别扭。

```shell
for((assignment;condition:next));do
    command_1;
    command_2;
    commond_..;
done;
```

如上所示，这里的 for 循环与 C 中的相似，但并不完全相同。

通常情况下 shell 变量调用需要加 $,但是 for 的 (()) 中不需要,下面来看一个例子：

```shell
#!/bin/bash
for((i=1;i<=5;i++));do
    echo "这是第 $i 次调用";
done;
```

执行结果：

```
这是第1次调用
这是第2次调用
这是第3次调用
这是第4次调用
这是第5次调用
```

与 C 中相似，赋值和下一步执行可以放到代码之前循环语句之中执行，这里要注意一点：如果要在循环体中进行 for 中的 next 操作，记得变量要加 $，不然程序会变成死循环。

## while语句

while循环用于不断执行一系列命令，也用于从输入文件中读取数据；命令通常为测试条件。其格式为：

```shell
while condition
do
    command
done
```

以下是一个基本的while循环，测试条件是：如果int小于等于5，那么条件返回真。int从0开始，每次循环处理时，int加1。运行上述脚本，返回数字1到5，然后终止。

```shell
#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```

运行脚本，输出：

```
1
2
3
4
5
```

**以上实例使用了 Bash let 命令，它用于执行一个或多个表达式，变量计算中不需要加上 $ 来表示变量。**

while循环可用于读取键盘信息。下面的例子中，输入信息被设置为变量FILM，按<Ctrl-D>结束循环。

```shell
echo '按下 <CTRL-D> 退出'
echo -n '输入你最喜欢的网站名: '
while read FILM
do
    echo "是的！$FILM 是一个好网站"
done
```

运行脚本，输出类似下面：

```
按下 <CTRL-D> 退出
输入你最喜欢的网站名:菜鸟教程
是的！菜鸟教程 是一个好网站
菜鸟教程2
是的！菜鸟教程2 是一个好网站
```

## 无限循环

无限循环语法格式：

```shell
while :
do
    command
done
```

或者

```shell
while true
do
    command
done
```

或者

```shell
for (( ; ; ))
```

## until循环

until循环执行一系列命令直至条件为true时停止。

until循环与while循环在处理方式上刚好相反。

一般 while 循环优于 until 循环，但在某些时候—也只是极少数情况下，until 循环更加有用。

until 语法格式:

```shell
until condition
do
    command
done
```

condition 一般为条件表达式，如果返回值为false，则继续执行循环体内的语句，否则结束循环。

以下实例使用util命令输出0~9的数字：

```shell
#!/bin/bash

a=0

until [ $a -gt 9]
do 
	echo $a
	a=`expr $a + 1`
done
```

## case

Shell case语句为多选择语句。可以用case语句匹配一个值与一个模式，如果匹配成功，执行相匹配的命令。case语句格式如下：

```shell
case 值 in
模式1)
	command1
	...
	commandN
	;;
模式2)
	command1
	...
	commandN
	;; ## 相当于break
esac
```

​	case工作方式如上所示。**取值后面必须为单词in**，每一**模式必须以右括号结束**。**取值可以为变量或常数**。匹配发现取值符合某一模式后，其间所有命令开始执行直至  **;;** 。

​	取值将检测匹配的每一个模式。一旦模式匹配，则执行完匹配模式相应命令后不再继续其他模式。**如果无一匹配模式，使用星号 * 捕获该值，再执行后面的命令。**

下面的脚本提示输入1到4，与每一种模式进行匹配：

```shell
echo '输入 1 到 4 之间的数字:'
echo '你输入的数字为:'
read aNum
case $aNum in
    1)  echo '你选择了 1'
    ;;
    2)  echo '你选择了 2'
    ;;
    3)  echo '你选择了 3'
    ;;
    4)  echo '你选择了 4'
    ;;
    *)  echo '你没有输入 1 到 4 之间的数字' # 相当于default
    ;;
esac
```

输入不同的内容，会有不同的结果，例如：

```shell
输入 1 到 4 之间的数字:
你输入的数字为:
3
你选择了 3
```

## 跳出循环

​	在循环过程中，有时候需要在未达到循环结束条件时强制跳出循环，Shell使用两个命令来实现该功能：**break和continue。**

### break

**break命令允许跳出所有循环（终止执行后面的所有循环）。**

下面的例子中，脚本进入死循环直至用户输入数字大于5。要跳出这个循环，返回到shell提示符下，需要使用break命令。

```shell
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字:"
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的! 游戏结束"
            break
        ;;
    esac
done
```

执行以上代码，输出结果为：

```
输入 1 到 5 之间的数字:3
你输入的数字为 3!
输入 1 到 5 之间的数字:7
你输入的数字不是 1 到 5 之间的! 游戏结束
```

### continue

continue命令与break命令类似，只有一点差别，**它不会跳出所有循环，仅仅跳出当前循环**。

对上面的例子进行修改：

```shell
#!/bin/bash
while :
do
    echo -n "输入 1 到 5 之间的数字: "
    read aNum
    case $aNum in
        1|2|3|4|5) echo "你输入的数字为 $aNum!"
        ;;
        *) echo "你输入的数字不是 1 到 5 之间的!"
            continue
            echo "游戏结束"
        ;;
    esac
done
```

运行代码发现，当输入大于5的数字时，该例中的循环不会结束，语句 **echo "游戏结束"** 永远不会被执行。

## case ... esac

**case ... esac** 与其他语言中的 switch ... case 语句类似，是一种多分枝选择结构，每个 case 分支用右圆括号开始，用两个分号 ;; 表示 break，即执行结束，跳出整个 case ... esac 语句，esac（就是 case 反过来）作为结束标记。

case ... esac 语法格式如下：

```shell
case 值 in
模式1)
    command1
    command2
    command3
    ;;
模式2）
    command1
    command2
    command3
    ;;
*)
    command1
    command2
    command3
    ;;
esac
```

case 后为取值，值可以为变量或常数。

值后为关键字 in，接下来是匹配的各种模式，每一模式最后必须以右括号结束，模式支持正则表达式。

## 实例

```shell
#!/bin/sh

site="runoob"

case "$site" in
   "runoob") echo "菜鸟教程"
   ;;
   "google") echo "Google 搜索"
   ;;
   "taobao") echo "淘宝网"
   ;;
esac
```

输出结果为：

```
菜鸟教程
```

# Shell函数

linux shell 可以用户定义函数，然后在shell脚本中可以随便调用。

shell中函数的定义格式如下：

```shell
[ function ] funname [()] ## 中括号是可选项

{

    action;

    [return int;]

}
```

**说明：**

- 可以带function fun()  定义，也可以直接fun() 定义,不带任何参数。
- 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255)

下面的例子的函数就是省略所有中括号中的选项：

```shell
#!/bin/bash
myfun{
        echo "my first fun in shell"
}
echo "begin myfun"
myfun
echo "end myfun"
```

会发生报错，因为没有 **function** 或  **（）小括号** 提示myfun是一个函数，因此编译器会认为它是一个命令。

修改如下即可执行：

```shell
#!/bin/bash
function myfun [()]{ ## 在myfun后面加括号或者在myfun前面加function
        echo "my first fun in shell"
}
echo "begin myfun"
myfun
echo "end myfun"
```

下面举例带return语句的函数：

```shell
#!/bin/bash

funWithReturn(){
        echo "这个函数会对输入的两个数字进行相加"
        echo "输入第一个数字："
        read num1
        echo "输入第二个数字："
        read num2
        echo "两个数字分别为 ${num1} 和 ${num2}"
        return $(($num1 + $num2))
}
funWithReturn
echo "两数之和为：$?"
```

输出如下：

```shell
这个函数会对输入的两个数字进行相加
输入第一个数字：
2
输入第二个数字：
3
两个数字分别为 2 和 3
两数之和为：5
```

**函数返回值在调用该函数后通过 $? 来获得。**

$? 仅对其上一条指令负责，一旦函数返回后其返回值没有立即保存入参数，那么其返回值将不再能通过 $? 获得。

测试代码：

```shell
#!/bin/bash
function demoFun1(){
    echo "这是我的第一个 shell 函数!"
    return `expr 1 + 1`
}

demoFun1
echo $?

function demoFun2(){
 echo "这是我的第二个 shell 函数!"
 expr 1 + 1
}

demoFun2
echo $?
demoFun1
echo 在这里插入命令！
echo $?
```

输出结果：

```shell
这是我的第一个 shell 函数!
2
这是我的第二个 shell 函数!
2
0
这是我的第一个 shell 函数!
在这里插入命令！
0
```

调用 demoFun2 后，函数最后一条命令 expr 1 + 1 得到的返回值（$?值）为 0，意思是这个命令没有出错。所有的命令的返回值仅表示其是否出错，而不会有其他有含义的结果。

第二次调用 demoFun1 后，没有立即查看 $? 的值，而是先插入了一条别的 echo 命令，最后再查看 $? 的值得到的是 0，也就是上一条 echo 命令的结果，而 demoFun1 的返回值被覆盖了。

下面这个测试，连续使用两次 **echo $?**，得到的结果不同，更为直观：

```shell
#!/bin/bash

function demoFun1(){
    echo "这是我的第一个 shell 函数!"
    return `expr 1 + 1`
}

demoFun1
echo $?
echo $?
```

输出结果：

```shell
这是我的第一个 shell 函数!
2
0
```

**注意：所有函数在使用前必须定义。这意味着必须将函数放在脚本开始部分，直至shell解释器首次发现它时，才可以使用。调用函数仅使用其函数名即可。**

## 函数参数

在Shell中，调用函数时可以向其传递参数。在函数体内部，通过 $n 的形式来获取参数的值，例如，$1表示第一个参数，$2表示第二个参数...

带参数的函数示例：

```shell
#!/bin/bash
# author:菜鸟教程
# url:www.runoob.com

funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

输出结果：

```
第一个参数为 1 !
第二个参数为 2 !
第十个参数为 10 !
第十个参数为 34 !
第十一个参数为 73 !
参数总数有 11 个!
作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
```

注意，$10 不能获取第十个参数，获取第十个参数需要${10}。当n>=10时，需要使用${n}来获取参数。

另外，还有几个特殊字符用来处理参数：

| 参数处理 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| $#       | 传递到脚本或函数的参数个数                                   |
| $*       | 以一个字符串的形式显示所有向脚本或函数传递的参数             |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程ID号                                   |
| $@       | 作用与$*相同，每个参数以一个字符串形式显示                   |
| $-       | 显示shell使用的当前选项，与set命令功能相同                   |
| $?       | 显示最后一个命令的退出状态。0表示没有错误，其他任何值表明错误。（也可以用于获取函数最返回值） |

# Shell输入/输出重定向

大多数 UNIX 系统命令从你的终端接受输入并将所产生的输出发送回到您的终端。一个命令通常从一个叫标准输入的地方读取输入，默认情况下，这恰好是你的终端。同样，一个命令通常将其输出写入到标准输出，默认情况下，这也是你的终端。

重定向命令列表如下：

| 命令            | 说明                                               |
| --------------- | -------------------------------------------------- |
| command > file  | 将输出重定向到file                                 |
| command < file  | 将输入重定向到file                                 |
| command >> file | 将输出以追加的方式重定向到file                     |
| n > file        | 将文件描述符为n的文件重定向到file                  |
| n >> file       | 将文件描述符为n的文件以追加的方式重定向到file      |
| n >& m          | 将输出文件m和n合并                                 |
| n <& m          | 将输入文件n和m合并                                 |
| << tag          | 将开始标记 tag 和结束标记 tag 之间的内容作为输入。 |

***需要注意的是文件描述符 0 通常是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。***

## 输出重定向

重定向一般通过在命令间插入特定的符号来实现。特别的，这些符号的语法如下所示:

```shell
command1 > file1
```

上面这个命令执行command1然后将输出的内容存入file1。

注意任何file1内的已经存在的内容将被新内容替代。**如果要将新内容添加在文件末尾，请使用 >> 操作符。**

### 实例

执行下面的 who 命令，它将命令的完整的输出重定向在用户文件中(users):

```shell
$ who > users
```

执行后，并没有在终端输出信息，这是因为输出已被从默认的标准输出设备（终端）重定向到指定的文件。

你可以使用 cat 命令查看文件内容：

```shell
$ cat users
_mbsetupuser console  Oct 31 17:35 
tianqixin    console  Oct 31 17:35 
tianqixin    ttys000  Dec  1 11:33 
```

输出重定向会覆盖文件内容，请看下面的例子：

```shell
$ echo "菜鸟教程：www.runoob.com" > users
$ cat users
菜鸟教程：www.runoob.com
$
```

如果不希望文件内容被覆盖，可以使用 >> 追加到文件末尾，例如：

```shell
$ echo "菜鸟教程：www.runoob.com" >> users
$ cat users
菜鸟教程：www.runoob.com
菜鸟教程：www.runoob.com
$
```

## 输入重定向

和输出重定向一样，Unix 命令也可以从文件获取输入，语法为：

```shell
command1 < file1
```

这样，本来需要从键盘获取输入的命令会转移到文件读取内容。

注意：输出重定向是大于号(>)，输入重定向是小于号(<)。

### 实例

接着以上实例，我们需要统计 users 文件的行数,执行以下命令：

```shell
$ wc -l users
       2 users # 两行菜鸟教程...
```

也可以将输入重定向到 users 文件：

```shell
$  wc -l < users
       2 
```

**注意：上面两个例子的结果不同：第一个例子，会输出文件名；第二个不会，因为它仅从标准输入读取内容。**

```shell
command1 < infile > outfile
```

同时替换输入和输出，执行command1，从文件infile读取内容，然后将输出写入到outfile中。

## 重定向深入讲解

一般情况下，每个 Unix/Linux命令运行时都会打开三个文件：

- **标准输入文件**(stdin)：stdin的文件描述符为0，Unix程序默认从stdin读取数据。
- **标准输出文件**(stdout)：stdout 的文件描述符为1，Unix程序默认向stdout输出数据。
- **标准错误文件**(stderr)：stderr的文件描述符为2，Unix程序会向stderr流中写入错误信息。

默认情况下，command > file 将 stdout 重定向到 file，command < file 将stdin 重定向到 file。

如果希望 stderr 重定向到 file，可以这样写：

```shell
$ command 2 > file
```

如果希望 stderr 追加到 file 文件末尾，可以这样写：

```shell
$ command 2 >> file
```

上面的 **2** 表示标准错误文件(stderr)。

如果希望将 stdout 和 stderr 合并后重定向到 file，可以这样写：

```shell
$ command > file 2>&1

或者

$ command >> file 2>&1
```

如果希望对 stdin 和 stdout 都重定向，可以这样写：

```shell
$ command < file1 >file2
```

command 命令将 stdin 重定向到 file1，将 stdout 重定向到 file2。

------



```shell
$ command > file 2>&1
$ command >> file 2>&1
```

**这里的&没有固定的意思。**

**放在>后面的&，表示重定向的目标不是一个文件，而是一个文件描述符。**

换言之 2>1 代表将stderr重定向到当前路径下文件名为1的regular file中，而2>&1代表将stderr重定向到文件描述符为1的文件(即/dev/stdout)中，这个文件就是stdout在file system中的映射。

而`&>file`是一种特殊的用法，也可以写成 `>&file`，二者的意思完全相同，都等价于

```shell
>file 2>&1
```

此处&>或者>&视作整体，分开没有单独的含义

**顺序问题：**

```shell
find /etc -name .bashrc > list 2>&1
# 我想问为什么不能调下顺序,比如这样
find /etc -name .bashrc 2>&1 > list
```

这个是从左到右有顺序的

第一种

```shell
$ xxx > list 2>&1
```

先将要输出到stdout的内容重定向到文件，此时文件list就是这个程序的stdout，再将stderr重定向到stdout，也就是文件list

第二种

```shell
$ xxx 2>&1 > list
```

先将要输出到stderr的内容重定向到stdout，此时会产生一个stdout的拷贝，作为程序的stderr，而程序原本要输出到stdout的内容，依然是对接在stdout原身上的，因此第二步重定向stdout，对stdout的拷贝不产生任何影响

------

对于上面 '2>&1'，举个例子，比如说:

```
$ find /etc -names "*.txt"  >list 2>&1
```

从左往右执行，执行到 >list，此时的 stdout 为 list；而执行到 2>&1，表示 stderr 重定向到 stdout，这里也就是 list 文件。

因为  **[ find /etc -names "\*.txt" ]** 这条命令是错误的( -names 应该是 -name)。

本来要输出到终端屏幕的错误信息:  

```
find: unknown predicate `-names`
```

被重定向到了 stdout 也就是 list 文件中，所以屏幕不会出现错误信息，而是打印到了 list 文件中。

**cat list** 可以查看到 find: unknown predicate `-names'   就在里面。 

## Here Document

Here Document 是 Shell 中的一种特殊的重定向方式，**用来将输入重定向到一个交互式 Shell 脚本或程序。** 

它的基本的形式如下：

```shell
command << delimiter
    document
delimiter
```

它的作用是将两个 delimiter 之间的内容(document) 作为输入传递给 command。

**注意：**

- 结尾的delimiter 一定要顶格写，前面不能有任何字符，后面也不能有任何字符，包括空格和 tab 缩进。
- 开始的delimiter前后的空格会被忽略掉。

### 实例

在命令行中通过 **wc -l**  命令计算 Here Document 的行数：

```shell
$ wc -l << EOF
	第一行
	第二行
	第三行
EOF
3	# 结果输出为 3 行
$
```

也可以在脚本中使用 Here Document：

```shell
#!/bin/bash

cat << EOF
	第一行
	第二行
	第三行
EOF
```

结果输出为：

```shell
第一行
第二行
第三行
```

## /dev/null 文件

如果希望执行某个命令，但又不希望在屏幕上显示输出结果，那么可以将输出重定向到 /dev/null：

```shell
$ command > /dev/null
```

/dev/null 是一个特殊的文件，写入到它的内容都会被丢弃；如果尝试从该文件读取内容，那么什么也读不到。但是 /dev/null 文件非常有用，将命令的输出重定向到它，会起到"禁止输出"的效果。

如果希望屏蔽 stdout 和 stderr，可以这样写：

```shell
$ command > /dev/null 2>&1
```

> **注意：**0 是标准输入（STDIN），1 是标准输出（STDOUT），2 是标准错误输出（STDERR）。
>
> 这里的 **2** 和 **>** 之间不可以有空格，**2>** 是一体的时候才表示错误输出。

# **参考资料**

[Shell教程|菜鸟教程]: https://www.runoob.com/linux/linux-shell.html

[linux粘着位]: https://www.cnblogs.com/yuanshuang/p/5619157.html
[linux：SUID、SGID详解]: https://www.cnblogs.com/fhefh/archive/2011/09/20/2182155.html

