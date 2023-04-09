# Module: trap

## 1. 中断处理

1. CPU: 进入`stvec`,

### 1.1 寄存器

**M-Mode**

1. `mtvec`: 异常向量基址寄存器，保存异常处理函数入口；
2. `mepc`: 存储发生异常时 PC 值；
3. `mcause`: 存储发生异常的种类；
4. `mie`: 中断使能寄存器；
5. `mip`: 中断请求寄存器；
6. `mtval`: 保存 trap 附加信息;
7. `mscratch`:
8. `mstatus`: 跟踪和控制当前 CPU 操作状态；
   - SIE: S-Mode 下全局中断使能位；
   - MIE: M-Mode 下全局中断使能位；
   - SPIE: 中断前 SIE 值；
   - MPIE: 中断前 MIE 值；
   - SPP: S-Mode 下上一个特权级；
   - MPP：M-Mode 下上一个特权级；
     ![特权级变化时mstatus的变化](/docs/imgs/mstatus_change.jpg)

> `ecall` 指令
>
> 1. 从用户态进入内核态
> 2. `epc`寄存器存储`ECALL`指令本身地址

> `mret`指令
>
> 1. 将`pc`设置为`mpc`的值，从 M-mode 的异常返回

> `sret`指令
>
> 1. 将`pc`设置为`spc`的值，从 S-mode 的异常返回

## 2. Functions

### 2.1 trap_init

```C
void trap_init(void)
```

- 功能：初始化异常/中断处理设置
  - 初始化异常向量表
- 输入：
- 输出：

### 2.2 trap_handler

```C
void trap_handler(long trap_type, long trap_addr, struct trap_context)
```

- 功能：异常/中断处理函数，判断 trap 类型
  - 系统调用：处理系统调用
  - ...
- 输入：
  - trap_type：trap 类型
  - trap_addr：trap 触发的地址
  - trap_context: 触发 trap 时的处理器上下文，包含关键寄存器信息
- 输出：
