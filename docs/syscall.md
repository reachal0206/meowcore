# Module: systemcall

功能：为用户态提供内核的各种功能；
比赛提供的系统调用方式遵循 RISC-V ABI，使用`ecall`指令，调用号存放在`a_7`寄存器中，6 个参数分别存储在 `a_0-a_5`寄存器中，返回值保存在 `a_0`中；
参考 linux 5.10 syscalls

> ecall 指令
>
> 1. 从用户态进入内核态
> 2. `epc`寄存器存储`ECALL`指令本身地址

## 1.Struct

## 2.Functions

### 2.1 syscall

```C
void syscall(long syscall_id, long *args)
```

- 功能：处理系统调用
- 输入：
  - `syscall_id`：系统调用号
  - `args`：参数列表
- 输出：该系统调用的返回值

### sys_call_table

```C
// 文件系统相关
#define SYS_getcwd 17
#define SYS_pipe2 59
#define SYS_dup 23
#define SYS_dup3 24
#define SYS_chdir 49
#define SYS_openat 56
#define SYS_close 57
#define SYS_getdents64 61
#define SYS_read 63
#define SYS_write 64
#define SYS_linkat 37
#define SYS_unlinkat 35
#define SYS_mkdirat 34
#define SYS_umount2 39
#define SYS_mount 40
#define SYS_fstat 80

// 进程管理相关
#define SYS_clone 220
#define SYS_execve 221
#define SYS_wait4 260
#define SYS_exit 93
#define SYS_getppid 173
#define SYS_getpid 172

//内存管理相关
#define SYS_brk 214
#define SYS_munmap 215
#define SYS_mmap 222
#define SYS_times 153
#define SYS_uname 160
#define SYS_sched_yield 124
#define SYS_gettimeofday 169
#define SYS_nanosleep 101


```
