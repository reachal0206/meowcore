# Module: thread

## Structs

### thread

```C
typedef thread{
  // struct context*
  // struct process*
  // kernel stack
  // execute status (new/ready/running/zombie/terminated)
  // exit status
}thread;
```

## Functions

### thread_create

```C
int thread_create(void *stack, int pc, void *args)
```

- 功能：创建一个新线程
  - create and init new TCB
  - attach the thread to its process
  - set args
- 输入：
  - stack：用户栈
  - pc
  - args
- 输出：

### thread_exit

```C
void thread_exit(int status)
```

- 功能：退出当前进程
  - set exit status
  - destroy thread context
  - remove current thread from process's list
  - exit process if it is the last thread
- 输入：
  - status: 退出状态
- 输出：
