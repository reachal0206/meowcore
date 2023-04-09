# Module: process

## Structs

### context

```C
typedef struct context{
}context;
```

处理器上下文结构体

### process

```C
typedef struct process{
  // pid
  // vmspace(include page table)
  // children process list
  // thread_list*
  // thread_count
}process;
```

进程结构体

## Functions

### process_create

```C
int process_create(char *file_path, char *argv[], char *envp[]){
  // alloc_process()
  // init_vmspace()
  // init page table

  // load and map binary file

  // create and map user stack
  // prepare user stack

  // create a thread
  // init thread_list
}
```

- 功能：创建一个新进程

### process_exit

```C
void process_exit(void){
  // destroy struct context
  // destroy vmspace
}
```

### process_clone

```C
void process_clone(int flags, int stack,int ptid, void tls, int ctid){
}
```

- 功能：创建一个子进程；
- 输入：
  - flags: 创建的标志，如 SIGCHLD；
  - stack: 指定新进程的栈，可为 0；
  - ptid: 父线程 ID；
  - tls: TLS 线程本地存储描述符；
  - ctid: 子线程 ID；
- 返回值：成功则返回子进程的线程 ID，失败返回-1；

### execve

```C
int execve(const char *path, char *const argv[], char *const envp[]){
}
```

- 功能：执行一个指定的程序；
- 输入：
  - path: 待执行程序路径名称，
  - argv: 程序的参数，
  - envp: 环境变量的数组指针
- 返回值：成功不返回，失败返回-1；

### process_wait

```C
int process_wait(int *status, int options){
}
```

- 功能：等待进程改变状态;
- 输入：
  - pid: 指定进程 ID，可为-1 等待任何子进程；
  - status: 接收状态的指针；
  - options: 选项：WNOHANG，WUNTRACED，WCONTINUED；
- 返回值：成功则返回进程 ID；如果指定了 WNOHANG，且进程还未改变状态，直接返回 0；失败则返回-1；

### process_exit

```C
void process_exit(int ec){
}
```

- 功能：触发进程终止，无返回值；
- 输入：
  - ec: 终止状态值；
- 返回值：无返回值；

### process_getppid

```C
void process_getppid(){
}
```

- 功能：获取父进程 ID；
- 输入：系统调用 ID；
- 返回值：成功返回父进程 ID；

### process_getpid

```C
void process_getpid(){
}
```

- 功能：获取进程 ID；
- 输入：系统调用 ID；
- 返回值：成功返回进程 ID；
