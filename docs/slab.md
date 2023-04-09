# Module: slab

## Structs

## Functions

### slab_init

```C
void* slab_init()
```

- 功能: 初始化 slab 分配器
  - 依次分配固定内存大小的资源池

```C
void* alloc_in_slab(int size)
```

- 功能: 在 slab 分配器中分配指定大小内存
  - 查找对应大小的资源池，分配内存
- 输入：
  - size: 内存大小
- 输出：

```C
void* free_in_slab(void *addr)
```

- 功能: 释放从 slab 分配器中获取的指定块
