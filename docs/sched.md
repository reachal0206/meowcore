# Module: sched

该模块为线程调度模块

## Module: RoundRobin

### roundrobin::sched_enqueue

```C
int roundrobin::sched_enqueue(struct thread* thread)
```

- 功能：将线程放入调度队列

### roundrobin::sched_dequeue

```C
int roundrobin::sched_dequeue(struct thread *thread)
```

- 功能：从调度队列中取出线程

## Structs

## Functions

### sys_yield

```C
void sys_yield(void)
```

- 功能：当前线程主动放弃资源切换到其他线程

### sys_yield

```C
void sys_yield(void)
```

- 功能：当前线程主动放弃资源切换到其他线程

### switch_context

```C
void switch_context(void)
```

- 功能：切换线程上下文

### sched_to_thread

```C
void sched_to_thread(struct thread* target_thread)
```

- 功能：调度到目标线程

### sched_to_thread

```C
void sched_to_thread(struct thread* target_thread)
```
