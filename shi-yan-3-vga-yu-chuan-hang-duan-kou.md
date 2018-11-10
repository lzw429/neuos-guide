# 实验3 VGA 与串行端口

## 实验目的

1. 了解 Linux 中 VGA 的实现，掌握 printk 等字符打印函数的实现方法。
2. 了解 Linux 通过串行端口与终端交换信息的函数实现。

## 实验内容

| 步骤 | 类型 | 内容 |
| :--- | :--- | :--- |
| Step1 | 编程 | 实现在屏幕指定位置输出字符的函数 video\_putchar\_at 与格式化输出内核态函数 printk |
| Step1.5 | 编程 | 实现滚屏函数 roll\_screen |
| Step2 | 编程 | 实现通过串行端口输出字符到终端的函数 s\_putchar 与通过串行端口格式化输出到终端的内核态函数 s\_printk |
| Step2.5 | 编程 | 实现通过串行端口读入信息的函数 |
| 拓展 | \(选做\)编程 | 实现有颜色功能的函数 echo |

### Step 1 视频图像阵列（VGA）

        打开 exp3/kernel/printk.c，该源文件中已有作为实验提示的详细注释。完成本任务需要依次实现两个函数：

* video\_putchar\_at
* printk

       在实现函数 printk 前，请仔细阅读实验资料中的“ 80 \* 25 彩色字符模式显示缓冲区的结构”。

{% hint style="info" %}
思考某屏幕坐标 \(x,y\) 对应的显存地址如何计算？其中，显存地址的首位在 exp3/kernel/printk.c 中由 video\_buffer 指针表示。
{% endhint %}

#### 函数 video\_putchar\_at\(char ch, int x, int y, char attr\)

        本函数用于在屏幕指定位置 \(x,y\) 处输出指定字符串 ch，并且指定字符串 ch 后光标的颜色 attr. 其中，x 是行数，y 是列数。

        ch 与 attr 变量不需要额外的处理，仅需要将这两个变量赋值到合适的内存地址处。

#### 不定参数函数

        在内核调用函数 printk 时，需要被格式化输出的内容长度是不定的，因此 printk 是一个不定参数函数。函数 printk 的实现中，将用到 &lt;stdarg.h&gt; 库中的下列数据结构或函数：

* va\_list
* va\_start
* va\_arg

{% hint style="info" %}
参考网站：[va\_arg、va\_copy、va\_end、va\_start](https://msdn.microsoft.com/zh-cn/library/kb57fad8.aspx)
{% endhint %}

#### 函数 printk\(char \*fmt, ...\)

        ****printf 与 printk 几乎一致，根据使用函数 printf 的经验，可知这个格式化输出函数的首个参数 char \*fmt 应当表示一段字符串的首位，以 \*fmt 为首的字符串描述了格式化输出的格式。

        这个字符串中可能含有以 “%” 为首位的子串，表示一个数字、一个char类字符或一段字符串，它具体的值即 printk 的某个在 \*fmt 之后的参数。

        请通过调用函数 printnum、video\_putchar、va\_start、va\_arg，尝试为函数 printk 实现如下功能：

1. 字符串无 “%” 时能直接在屏幕上输出字符串的内容
2. 读取到 “%d”，根据某个参数输出一个有符号十进制整数
3. 读取到 “%u”，根据某个参数输出一个无符号十进制整数
4. 读取到 “%x”，根据某个参数输出一个无符号十六进制整数
5. 读取到 “%c”，根据某个参数输出一个字符
6. 读取到 “%s”，根据某个参数输出一段字符串
7. 读取到 “%%”，输出百分号“%”

{% hint style="warning" %}
 “%” 可能是最后一个字符，即一个百分号可能没有后继的字符。
{% endhint %}

         若函数 video\_putchar\_at 与函数 printk 已成功实现，使用 QEMU 运行 neuos，将见到如图 3-1 的界面。printk 函数是由 exp3/kernel/main.c 调用的。

![&#x56FE; 3-1 &#x51FD;&#x6570; printk &#x6210;&#x529F;&#x5B9E;&#x73B0;](https://lh6.googleusercontent.com/vNw4oxlpILlV7H9cyP4SwdeprB8w6UjA41yjaOnytCTKjLcojhJIkyBb0yNFSivVoGUM8OEwpW7VwfKDdZG6lo-UGXYwFiCjGZWWuhprZeC5zFBaO2Pnaj07w6q3MfXhL2hPARts)

{% hint style="info" %}
参考网站：[Printing To Screen - OSDev Wiki](http://wiki.osdev.org/Printing_To_Screen)
{% endhint %}

### Step 1.5 滚屏函数 roll\_screen

     ****   尝试通过调用 printk.c 中的两个函数 memcpy 与 video\_putchar\_at 实现滚屏函数 roll\_screen 。

        滚屏函数被调用的条件应当是，需要输出字符到当前屏幕的最后一行之后。因此需要将当前屏幕的内容整体上移，使得屏幕最后一行能够输出新的内容。

        要验证函数 roll\_screen 是否成功实现，可打开目录 exp3/kernel/main.c，使用函数 printk 输出足够多的字符，再运行 neuos，测试能否成功滚屏。

### Step 2 串行端口

        进入 exp3/kernel 目录，打开 serial.c 源文件。

        函数 is\_transit\_empty 用于判断是否能够传输，若允许传输，函数返回 0.

        结合函数 is\_transit\_empty 与端口输出函数 outb，实现函数 s\_putchar. 参考 [Serial Ports - OSDev Wiki](http://wiki.osdev.org/Serial_Ports) 4.3 节。

        然后，类似于 Step 1，请通过调用函数 s\_printnum、s\_putchar、va\_start、va\_arg，尝试为函数 s\_printk 实现如下功能：

1. 字符串无 “%” 时能直接在屏幕上输出字符串的内容
2. 读取到 “%d”，根据某个参数输出一个有符号十进制整数
3. 读取到 “%u”，根据某个参数输出一个无符号十进制整数
4. 读取到 “%x”，根据某个参数输出一个无符号十六进制整数
5. 读取到 “%c”，根据某个参数输出一个字符
6. 读取到 “%s”，根据某个参数输出一段字符串
7. 读取到 “%%”，输出百分号“%”

{% hint style="warning" %}
再次当心， “%” 可能是最后一个字符。
{% endhint %}

        若函数 s\_putchar 与函数 s\_printk 已成功实现，使用 QEMU 运行 neuos，运行结果如图 3-2 所示，字符被输出到终端而不是 QEMU 界面中。

![&#x56FE; 3-2 &#x51FD;&#x6570; s\_printk &#x6210;&#x529F;&#x5B9E;&#x73B0;](https://lh4.googleusercontent.com/a-h2h7XOoENLfW1JHbMcXB5HiBdjrv3TrS32yOKb_va503aQjOnj8j85KoWTPAfGbzbmlfkoIHu4PyS0HAX5EkRqIwPprz6B7GZCA-K4fEjp7b7Uok4poEFyoXKUrK5jH1Jgf-Wr)

### **Step 2.5 读取信息**

        在 exp3/kernel/serial.c 源文件的末尾，尝试实现通过串行端口读取信息的代码。仅尝试完成代码，不必实现最终效果。

{% hint style="info" %}
参考：[Serial Ports - OSDev Wiki](http://wiki.osdev.org/Serial_Ports) 读取数据部分。
{% endhint %}

## 报告要点

1. 描述屏幕上指定坐标\(x, y\)的显存地址如何计算？
2. 展示实现 printk 函数的关键代码。
3. 展示函数实现后的运行效果。

## 参考资料

### **80 \* 25 彩色字符模式显示缓冲区的结构**

        内存地址中，B8000h ~ B8FFFFh 共 32KB 的空间，是 80\*25 彩色字符模式的显示缓冲区。在这个地址空间中写入数据，写入的内容会立即出现在显示器上。

        在 80\*25 彩色字符模式下，显示器可显示 25 行，每行 80 个字符，每个字符可以有 256种属性，包括背景色、前景色（即字体色）、闪烁、高亮等组合而成的属性。

        一个字符在显示缓冲区中占用 2 个字节，分别存放其 ASCII 码和属性。一屏的内容在显示缓冲区中共占用占 2 \* 25 \* 80 = 4000 字节。

        显示缓冲区分 8 页，每页 4 KB，显示器可显示任意一页的内容。一般地，显示第 0 页内容，即显示 B8000H ~ B8F9FH 中的共 4000 字节的内容。

        在一页显示缓冲区中，一行的 80 个字符占用 160 字节，偏移 000 ~ 09f 对应显示器上的第一行，偏移 0A0 ~ 13f 对应显示器上的第二行，以此类推。

        在属性字节中，闪烁、背景色、高亮与前景色是按位设置的，如表 3-1.

#### 表 3-1 属性字节中每位的具体含义

| 7 | 6 | 5 | 4 | 3 | 2 | 1 | 0 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| BL | R | G | B | I | R | G | B |
| 闪烁 | 背景色 | 背景色 | 背景色 | 高亮 | 前景色 | 前景色 | 前景色 |

{% hint style="info" %}
前景色即字体色。
{% endhint %}

### **视频图形阵列（VGA）**

      ****  视频图形阵列（Video Graphics Array，简称 VGA）是 IBM 于 1987 年提出的一种电脑显示标准。运用该标准的接口被称为 VGA 端子，通常用于在电脑的显示卡、显示器及其他设备发送模拟信号。

### **串行端口**

 ****       串行端口（Serial port）主要用于串行式逐位数据传输。可用于连接外置调制解调器、打印机、路由器等设备。在消费电子领域已被 USB 替代，在网络设备中仍是主要的传输控制方式。

        通过串行接口，Linux 设备，即使并非计算机，也可实现与其他设备的通信；本实验中的串行端口用于将 neuos 中的信息打印到 NEU-OS Lab Environment 的终端中。

## **拓展学习**

        ****找到 printk.c 源文件 208 行以后的 “拓展学习” 部分，实现具有指定 8 种颜色功能的函数 echo；echo 需要调用 video\_putchat\_color 等函数。

{% hint style="info" %}
实现效果参考：[Bash tips: Colors and formatting](http://misc.flogisoft.com/bash/tip_colors_and_formatting)
{% endhint %}

      ****  例如，echo\(“\033\[x;ymneuos”\); 可将 “neuos” 以 x 为前景色，y 为背景色输出。其中 x 和 y 是两个数字，均是 0~7 的整数，其表示的颜色见表 3-2。m 作为颜色的唯一后缀。

        如果没有 “\033\[” 指定颜色，函数 echo 应直接输出文本。这个格式是严格的，不符合这个格式的字符串应按普通文本直接输出。函数应支持输出多个指定颜色的字符串。

#### 表 3-2 前景色与背景色代码表示的颜色

| x：字体色 | y：背景色 | 颜色 | RGB 二进制 |
| :--- | :--- | :--- | :--- |
| 0 | 0 | BLACK | 000 |
| 1 | 1 | RED | 100 |
| 2 | 2 | GREEN | 010 |
| 3 | 3 | YELLOW | 110 |
| 4 | 4 | BLUE | 001 |
| 5 | 5 | MAGENTA | 101 |
| 6 | 6 | CYAN | 011 |
| 7 | 7 | WHITE | 111 |

      ****  表 3-2 中的 RGB 指的是表 3-1 中属性字节的第 0～2 位及第 4～6 位；为了更好的显示效果，此处指定属性字节的**第 3 位置为 1，第 7 位置为 0**. 根据这些信息，并结合表 3-1，可计算出应填充到属性字节的十六进制数。

        考虑到实现的难度等因素，此处的要求与上述参考网站提到的并不完全相同。

        如要测试该函数是否正确实现，在 exp3/kernel/main.c 中调用函数 echo 即可。

![&#x56FE; 3-3 &#x51FD;&#x6570; echo &#x5B9E;&#x73B0;&#x6548;&#x679C;](https://lh4.googleusercontent.com/w6AelVCZCQewlrd4m7Nd2mPwxCVJGihY9prTXO6dDEpsp1qX5e29vVuCd1o05afqbP5PsDW9_L1XiN6yvFiISrkgBO8vj2RVA26oXHu7J7p2cs-_ejv3t0HndjDwxdCStOHC1GgC)

{% hint style="info" %}
C 语言中‘\\’是斜杠的转义字符，’\0’是 NULL 的转义字符。
{% endhint %}

