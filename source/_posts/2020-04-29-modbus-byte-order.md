---
title: Modbus - Byte Order
date: 2020-04-29 13:14
categories: IOT
tags:
- modbus
- byte order
thumbnail: "https://tse4-mm.cn.bing.net/th/id/OIP-C.UtbZIbpIwpAc4Ub2JHnZYQHaBn?rs=1&pid=ImgDetMain"
---

##### 高地址 低地址
![image](https://img-blog.csdn.net/20180923182938492?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29xcUh1VHUxMjM0NTY3OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
##### 高字节 低字节
如int a=16777220，化为十六进制是0x01000004则04属于低字节，01属于高字节

##### 大小端模式
（1）如果a在内存中的存放顺序为下图（即低字节存放在高地址），则为大端模式

![image](https://img-blog.csdn.net/20180923183740810?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29xcUh1VHUxMjM0NTY3OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

（2）如果a在内存中的存放顺序为下图（即低字节存放在低地址），则为小端模式

![image](https://img-blog.csdn.net/20180923183653465?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L29xcUh1VHUxMjM0NTY3OA==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### （3）如何互换（通过移位操作再或）

```
For 2 byte data (16bit)

#define BigtoLittle16(A)    (( ((uint16)(A) & 0xff00) >> 8) | \
                            (( (uint16)(A) & 0x00ff) << 8))

For 4 byte data (32bit)

#define BigtoLittle32(A)    (( (uint32)(A) & 0xff000000 >> 24) | \
                            (( (uint32)(A) & 0x00ff0000) >> 8) | \
                            (( (uint32)(A) & 0x0000ff00) << 8) | \
                            (( (uint32)(A) & 0x000000ff) <<24))
```

----

字节顺序是指多字节数据在计算机内存中存储或者网络传输时各字节的存储顺序。

> Consider a 16-bit integer that is made up of 2 bytes. There are two ways to store the two bytes in memory: with the low-order byte at the starting address, known as little-endian byteorder, or with the high-order byte at the starting address, known as big-endian byte order.

##### 1. 大端字节序（==Big Endian==)
最高有效位（MSB：Most Significant Bit）存储于最低内存地址处；

最低有效位（LSB：Lowest Significant Bit）存储于最高内存地址处。

##### 2. 小端字节序（==Little Endian==）
最高有效位（MSB：Most Significant Bit）存储于最高内存地址处；

最低有效位（LSB：Lowest Significant Bit）存储于最低内存地址处。

##### 主机字节序
不同的主机有不同的字节序，如x86为小端字节序，Motorola 6800为大端字节序，ARM字节序是可配置的。

##### 网络字节序
网络字节顺序是TCP/IP中规定好的一种数据表示格式，它与具体的CPU类型、操作系统等无关，从而可以保证数据在不同主机之间传输时能够被正确解释。网络字节顺序采用**big endian**排序方式。

----

为使网络程序具有可移植性，使同样的C代码在大端和小端计算机上编译后都能正常运行，可以调用以下库函数做网络字节序和主机字节序的转换。

```
#include <arpa/inet.h>  
uint32_t htonl(uint32_t hostlong);  
uint16_t htons(uint16_t hostshort);  
uint32_t ntohl(uint32_t netlong);  
uint16_t ntohs(uint16_t netshort);
```

这些函数名很好记:

- h表示host
- n表示network
- l表示32位长整数
- s表示16位短整数

例如htonl表示将32位的长整数从主机字节序转换为网络字节序，例如将IP地址转换后准备发送。如果主机是小端字节序，这些函数将参数做相应的大小端转换然后返回，如果主机是大端字节序，这些函数不做转换，将参数原封不动地返回。

[《UNIX网络编程 卷1：套接字联网API（第3版）》](https://link.zhihu.com/?target=https%3A//github.com/BeginMan/BookNotes/tree/master/Unix/Unix-Network-Programming-Volume-1-The-Sockets-Networking-API-3rd-Edition)中检查主机的大端小端程序如下：

```
#include "unp.h"
 
int main(int argc, char **argv)
{
    union {
        short       s;
        char        c[sizeof(short)];
    } un;
 
    un.s = 0x0102;                                  //短整数变量中存放2个字节的值0x0102
    printf("%s: ", CPU_VENDOR_OS);                  //标识CPU类型，厂家和操作系统版本，如:i386-apple-darwin14.5.0
    if(sizeof(short) == 2) {
        //查看它的两个连续字节c[0],c[1]来确定字节序
        if (un.c[0] == 1 && un.c[1] == 2)
            printf("big-endian\n");
        else if (un.c[0] == 2 && un.c[1] == 1)
            printf("litter-endian\n");
        else
            printf("unknown\n");
    } else
        printf("sizeof(short) = %lu\n", sizeof(short));
    exit(0);
}
```

还有一个更加简明的方式，如下：

```
#include <stdio.h>
#include <arpa/inet.h>
 
int main(void)
{
    unsigned int x = 0x12345678;
    unsigned char *p = (unsigned char *) &x;
    printf("%x,%x,%x,%x\n", p[0], p[1], p[2], p[3]);
 
    unsigned int y = htonl(x);
    p = (unsigned char *)&y;
    printf("%x,%x,%x,%x\n", p[0], p[1], p[2], p[3]);
    return 0;
}
```

输出：

```
78 56 34 12  
12 34 56 78
```

即本主机是小端字节序，而经过htonl转换后为网络字节序，即大端。