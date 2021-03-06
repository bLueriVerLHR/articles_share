# 控制台输出颜色控制

原文链接：<https://cloud.tencent.com/developer/article/1142372>

---

通用的控制文本颜色的转义序列格式如下：

``` bash
CSI n1 [;n2 [;…]] m
```

其中CSI全称为“控制序列引导器”（Control Sequence Introducer/Initiator），也就是 `\033[`（其中 `\033` 是你键盘左上角 `Esc键` 对应的ascii码（八进制））；

n1、n2等表示SGR参数（下面会列出一些常用的SGR参数），用于控制颜色、粗体、斜体、闪烁等文本输出格式；m表示转义序列结束。

## 颜色参数

``` bash
格式：\033[显示方式;前景色;背景色m

说明：
前景色            背景色           颜色
---------------------------------------
30                40              黑色
31                41              红色
32                42              绿色
33                43              黃色
34                44              蓝色
35                45              紫红色
36                46              青蓝色
37                47              白色

显示方式           意义
-------------------------
0                终端默认设置
1                高亮显示
4                使用下划线
5                闪烁
7                反白显示
8                不可见

例子：
\033[1;31;40m    # 1-高亮显示 31-前景色红色  40-背景色黑色
\033[0m          # 采用终端默认设置，即取消颜色设置
```

![常用颜色](../_media/pics/colorful_console/colorful_console_1.png)

![RGB颜色](../_media/pics/colorful_console/colorful_console_2.png)

RGB颜色用法：

``` bash
ESC[ … 38;2;<r>;<g>;<b> … m Select RGB foreground color
ESC[ … 48;2;<r>;<g>;<b> … m Select RGB background color
```
特别注意：此处用法需从 `38` 开始设置, 代表前景色，`48` 代表后景色；`2` 代表RGB模式，后面三位为 `RGB` 数值。前后仍可使用 `SRG参数`。

例子：

``` bash
echo  -e "\033[1;38;2;255;85;85mI ♡  You \e[0m"
```

> 附：经测试，在不是用 `SRG参数` 的情况下，可将 `;` 替换为 `:` 但在使用 `SRG参数` 时无效。 即：echo  -e "\033[38:2:255:85:85mI ♡  You \e[0m"

![256色模式](../_media/pics/colorful_console/colorful_console_3.png)

使用方法：

``` bash
ESC[ … 38;5;<n> … m Select foreground color
ESC[ … 48;5;<n> … m Select background color
```

例子

``` bash
echo  -e "\033[1;38;5;9mI ♡  You \e[0m"
```

## SRG控制参数

|数码|作用|
|-|-|
| 0  |   关闭所有格式，还原为初始状态 |
| 1  |    粗体/高亮显示    |
| 2  |     模糊（※）  |
| 3  |     斜体（※）  |
| 4  |     下划线（单线）  |
| 5  |     闪烁（慢）  |
| 6  |     闪烁（快）（※） |
| 7  |     交换背景色与前景色  |
| 8  |     隐藏（伸手不见五指，啥也看不见）（※）|

1. 其中含有（※）标注的编码表示不是所有的终端仿真器都支持，只有少数仿真器支持。
2. 多个 `SGR参数` 可以组合使用，例如：`echo -e "\x1b[31;4mRed Underline Text\e[0m"` 输出红色下划线字体 `Red Underline Text`。
3. 更多参数信息请参考 [ANSI escape code](https://en.wikipedia.org/wiki/ANSI_escape_code)。