# code reading
## 如何实现时钟中断
    当timer到达timeout的时候，reset timer
    如果中断使能开，则设置中断类型为时钟中断
    关中断，跳转到interrup服务程序
    interrupt处理。。。没看懂。。。
    清除时钟中断

## 关键变量有误/不全的地方
 - a: a寄存器
 - b: b寄存器
 - c: c寄存器
 - f: f浮点寄存器
 - g: g浮点寄存器
 - ir:　指令寄存器
 - xpc: pc在host内存中的值
 - fpc: pc在host内存中所在页的下一页的起始地址值
 - tpc: pc在host内存中所在页的起始地址值
 - xsp: sp在host内存中的值
 - tsp: sp在host内存中所在页的起始地址值
 - fsp: 辅助判断是否要经过tr/tw的分析
 - ssp: system stack pointer 核心态的栈指针
 - usp: user stack pointer 
 - cycle: xcycle / 4
 - xcycle: pc超过xcycle的时候，取回下一页？？
 - timer: 时钟，时钟中断所用
 - timeout: 时钟计时到达timeout的时候，触发时钟中断
 -　detla: 一个页表项的大小？
 
 ## 有误？

“有两个指针tr/tw, tw指向内核态或用户态的read/write　page translation table．”
少了一个tr?


## 在v9-cpu中的跳转相关操作是如何实现的
1.修改xcycle为跳转后的xcycle，修改pc到跳转后的pc
2.判断是否超出pc所在页，如果是转fixpc
3.转next
fixpc的含义：
    检查是否有page fault。
    如果有，转到异常处理
    把xcycle和fpc更新到新的一页
next的含义：
    下一条指令开始
        检查是否有键盘中断
        检查是否有时钟中断
        继续执行下一条指令

## 在v9-cpu中如何设计相应指令，可有效实现函数调用与返回；

## emhello/os0/os1等程序被加载到内存的哪个位置,其堆栈是如何设置的；

## 在v9-cpu中如何完成一次内存地址的读写的；
    1.判断是否在当前页中，如果是，则直接赋值，结束，否则转2
    2.如果

## 在v9-cpu中如何实现分页机制；
    使用tr tw列表 得到实页号v = tr[(v = (uint)xpc - tpc) >> 12]，在rlook/wlook是否页缺失，如果是，触发一个page fault exception


请编写一个小程序，在v9-cpu下，能够接收你输入的字符并输出你输入的字符

[x]
请编写一个小程序，在v9-cpu下，能够产生各种异常/中断

[x]
　
请编写一个小程序，在v9-cpu下，能够统计并显示内存大小

[x]
　