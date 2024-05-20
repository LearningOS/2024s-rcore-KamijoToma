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
