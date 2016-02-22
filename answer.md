# code reading
## 如何实现时钟中断
    当timer到达timeout的时候，reset timer
    如果中断使能开，则设置中断类型为时钟中断
    关中断，跳转到interrup服务程序
    interrupt处理。。。没看懂。。。
    清除时钟中断

## 关键变量有误/不全的地方
 - ssp: system stack pointer 核心态的栈指针
 - usp: user stack pointer 
 - cycle: xcycle / 4
 - xcycle: pc超过xcycle的时候，取回下一页？？
 - timer: 时钟，时钟中断所用
 - timeout: 时钟计时到达timeout的时候，触发时钟中断
 -　detla: 一个页表项的大小？
 

```
???
“有两个指针tr/tw, tw指向内核态或用户态的read/write　page translation table．”
少了一个tr?
```

## 在v9-cpu中的跳转相关操作是如何实现的
1.修改xcycle为跳转后的xcycle，修改pc到跳转后的pc
2.判断是否超出pc所在页，如果是转fixpc
3.转next

```
???
fixpc的含义：
    检查是否有page fault。
    如果有，转到异常处理
    把xcycle和fpc更新到新的一页```
```
???
xcycle:
```
next的含义：
    下一条指令开始
        检查是否有键盘中断
        检查是否有时钟中断
        继续执行下一条指令

## 在v9-cpu中如何设计相应指令，可有效实现函数调用与返回；
    把函数参数依次压入栈

```???why +8 / 16```

## emhello/os0/os1等程序被加载到内存的哪个位置,其堆栈是如何设置的；
自底向上

## 在v9-cpu中如何完成一次内存地址的读写的；
```
    case LL:   
    // 在页内，不用进行虚实转换 
    if (ir < fsp) { a = *(uint *)(xsp + (ir>>8)); continue; }
    // 如果tlb没有命中 且 页表里面也没有 -> break，否则，虚实页地址转换
    ??? v^p????是什么？
               if (!(p = tr[(v = xsp - tsp + (ir>>8)) >> 12]) && !(p = rlook(v))) break; a = *(uint *) ((v ^ p) & -4);
               // 如果fsp越界，超出页表 
               // ??? 为什么这里xsp会变？
               if (fsp || (v ^ (xsp - tsp)) & -4096) continue; goto fixsp;
```
    1.判断是否在当前页中，如果是，则直接赋值，结束，否则转2
    2.goto fixsp
```
???为什么switch case后的指令都是continue？而不是break？
```

## 在v9-cpu中如何实现分页机制；
    TLB: 使用tr tw列表 得到实页号v = tr[(v = (uint)xpc - tpc) >> 12]，
    页表转换：在rlook/wlook是否页缺失，如果是，触发一个page fault exception


