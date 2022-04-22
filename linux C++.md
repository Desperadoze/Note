# linux C++

## 基本概念

### gcc编译过程



![image-20220414160926281.png](https://pic.dogimg.com/2022/04/18/625d220dc1872.png)

输入不同参数以直接执行靠后的指令，前面的过程都会被默认执行

* 预处理：-E，以.i结尾

预处理器：头文件展开，宏替换、删掉注释

* 编译：-S	以.s结尾

* 汇编：-C  以.o结尾

​	使用./name运行可执行文件

* -D 指定一个宏，调试用

```c
#ifdef NAME
	//printf（）
#endif
```

* -Wall 生成所有警告信息

* -On 优化级别0-3四级
* -l 指定使用的库

---

### 静态库的制作

静态库：链接阶段复制到程序中

动态库：运行时系统动态加载到内存中供程序调用

**命名规则** 

Linux：libxxx.a

​		lib：前缀

​		xxx：库名，自己起

​		.a 后缀

![image-20220414163715487.png](https://pic.dogimg.com/2022/04/18/625d223a15cd2.png)



### 动态库的制作

![image-20220414165153156.png](https://pic.dogimg.com/2022/04/18/625d223b82874.png)

![image-20220414165249833.png](https://pic.dogimg.com/2022/04/18/625d223cb56b2.png)



### makefile

目标：依赖

​		命令

`make`

##### 自定义变量

变量名=变量值  var=hello

##### 预定义变量

AR：归档维护程序，默认值为ar

CC：C compiler，默认值cc

CXX：g++

$@:目标的完整名称

$<:第一个依赖文件的名称

$^:所有的依赖文件

##### 获取变量的值

$变量名

#### 模式匹配

% 通配符

#### 函数

##### $(wildcard PATTERN...)

##### $(patsubst <source>,<target>,$(src))

##### 伪目标

不会和外界文件进行对比



### GDB调试

`-g`

`Wall`打开所有警告

`q  quit`退出

`gcc -g -Wall program.c -o -program`

启动：`gdb 可执行文件`

设置/获取参数：set、show

`list`查看代码

查看非当前文件代码

`list 文件名：行号`,也可以接函数名

默认显示10行

`set list num`设置行数

##### 断点  断点行不执行

- 设置断点`b/break 行号`

- 查看断点`info break`

> 打断点在函数或者别的文件中也可，和list相似

==info可以简写为i，break也可简写为b==

- 删除断点`delete 断点号（用info去查）`

- 设置断点无效`disable/dis`
- 设置断点有效`enable/ena`

- 条件断点`break 10 if i==5`   一般用在循环的位置

##### GDB调试命令

- `start `停在第一行
- `run` 遇到断点才停
- `c/continue` 继续运行，到下一个断点停
- `n/next` 向下执行一行代码（不会进入函数体）
- `p/print `变量
- `ptype `变量
- `s/step `向下单步调试（会进入函数体）
- `finish `跳出函数体，函数中不能有断点
- `display num`自动打印指定变量的值
- `i/info display`
- `undisplay num`
- `set var` 变量名=变量值
- `until` 跳出循环

### 文件IO

标准C库会通过FILE文件指针写入buffer

Linux系统IO会直接传，网络通信使用这个

