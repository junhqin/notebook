### 典型计算机病毒分析

Yankee Doodle病毒（文件病毒）

* 病毒特征、表现形式及危害
* 病毒的传染途径 (可执行文件加载运行的时候来进行病毒传染，)
* 病毒运行机制

![image-20231122103652007](./assets/image-20231122103652007.png)

int:调用DOS中断,4bh是子功能部分

感染可执行文件，在可执行文件加载运行的时候进行传染

攻击传染部分（int21h/4bh）截取该中断，在DOS系统。

<<<<<<< HEAD
感染后，c6h中断服务程序会将调用STC,将CF置为1



攻击传染部分

![image-20231122221601413](./assets/image-20231122221601413.png)
=======
在数组程序加载的时候都有机会调用到攻击传染部分，这时候就可以进行判断病毒是否被传染

int 1ch时钟中断服务程序

驻留的判断：

```
clc  #清楚进位标志CF

mov ax，c603h  #调用了c6h子功能

int 21h
```

c6h功能程序会被病毒修改成以下指令：

```
stc  #s

iret  #中断程序返回
```


---
>>>>>>> 56d05080f0fdb70e92ddb74f450dbc2d0a292c51
