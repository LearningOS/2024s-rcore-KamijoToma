# Lab1 Report

## 总结

1. 向系统调用路由函数加入了一个统计函数 `count_syscall(syscall_id)` 用于统计系统调用信息
2. 在process模块中加入了四个函数，用于处理系统调用统计、返回初次调度时间等功能
3. 为进程调度块添加了两个属性，用于记录系统调用统计和初次调度事件

## 简答题

### 1

内核收到了PageFault IllegalInstruction LoadFault异常。

```
[kernel] PageFault in application, bad addr = 0x0, bad instruction = 0x804003ac, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] IllegalInstruction in application, kernel killed it.
[kernel] Panicked at src/trap/mod.rs:72 Unsupported trap Exception(LoadFault), stval = 0x18!
```

SBI版本 `RustSBI-QEMU Version 0.2.0-alpha.2`

### 2

#### 2.1

`a0` 代表内核栈基地址。`__restore`函数有两种使用情境，一种是从TrapHandler返回，另一种是第一次运行程序时

#### 2.2

三个寄存器 `sstatus` `sepc` `sscratch`

| sstatus | sepc | sscratch |
:--: | :--: | :---:
| 保存CPU状态 | 记录触发中断的指令地址 | 在保存和恢复栈的过程中作为一个特殊的中转寄存器 |


#### 2.3

sp和tp寄存器未被保存。tp寄存器除非有特殊用途，否则不需要保存。

sp是栈寄存器，在L13已经被切换为内核栈地址，因此不能在此刻保存，需要保存sscratch寄存器。

#### 2.4

sp为内核栈地址，sscratch为用户栈地址

#### 2.5

sret指令。该指令在硬件上修改CPU状态为用户态，并返回到sscratch指向的栈地址

#### 2.6

sp为内核栈地址，sscratch为用户栈地址

#### 2.7

用户程序中的 `ecall` 指令

## 荣誉准则



1. 在完成本次实验的过程（含此前学习的过程）中，我曾分别与 无 就（与本次实验相关的）以下方面做过交流，还在代码中对应的位置以注释形式记录了具体的交流对象及内容：

2. 此外，我也参考了 rCore 文档和源代码 ，还在代码中对应的位置以注释形式记录了具体的参考来源及内容：

3. 我独立完成了本次实验除以上方面之外的所有工作，包括代码与文档。 我清楚地知道，从以上方面获得的信息在一定程度上降低了实验难度，可能会影响起评分。

4. 我从未使用过他人的代码，不管是原封不动地复制，还是经过了某些等价转换。 我未曾也不会向他人（含此后各届同学）复制或公开我的实验代码，我有义务妥善保管好它们。 我提交至本实验的评测系统的代码，均无意于破坏或妨碍任何计算机系统的正常运转。 我清楚地知道，以上情况均为本课程纪律所禁止，若违反，对应的实验成绩将按“-100”分计。
