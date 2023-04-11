# Module: slab

为了满足操作系统分配小内存对象的需求，将从伙伴系统分配的大块内存细分为固定大小的小内存块进行管理。内存块的大小一般为$2^n$次方，在`MeowCore`中$2<n<12$。
![slub分配器](/docs/pics/slub分配器.jpg)

## Structs

### slab

```C
typedef slab{
  void *free_list;
  slab *next_slab;
  int order;
}slab_head;
```

### slot

```C
typedef slot{
  slot *next_free;
}slot;
```

## Variables

`SLAB_MIN_ORDER=3`
`SLAB_MAX_ORDER=11`
`SLAB_INIT_SIZE=`
`slab* slabs[SLAB_MAX_ORDER-SLAB_MIN_ORDER+1]`

## Functions

### slab_init

```C
void* slab_init(){

  // for order in range(SLAB_MIN_ORDER)->SLAB_MAX_ORDER:
  // init_slab_cache(order, SLAB_INIT_SIZE)
}
```

- 功能: 初始化 slab 分配器
  - 依次分配固定内存大小的资源池

### init_slab_cache

```C
void* init_slab_cache(int order, int size){
  // 从buddy system中获取大小为size的内存分配给slabs[order-SLAB_MIN_ORDER]
  // slabs[order-SLAB_MIN_ORDER]->next_slab=NULL
  // 将slabs[order-SLAB_MIN_ORDER]的内存均分为大小为2^order的块，从第二个内存块开始所有内存块加入slabs[order-SLAB_MIN_ORDER]->free_list
}
```

- 功能: 初始化大小为$2^n$的内存块组成的内存资源池 slab_cache
  - 向伙伴系统申请大小为`SLAB_INIT_SIZE`的物理内存块作为 slab_cache 放入 slabs 数组；
  - 将 slab_cache 划分为等长$(2^{order})$的小块内存，将其内部空闲的小块内存组织为空闲链表；
  -

```C
void* alloc_in_slab(int size){
  // 找到适合大小(2^order)的内存块
  // 从slabs[order]的slab链表中寻找有空闲slot的slab
  // 找到后将空闲slot移出对应free_list
  // 否则调用init_slab_cache在slabs[order]的slab链表末尾添加空闲slab
}
```

- 功能: 在 slab 分配器中分配指定大小内存
  - 查找对应大小的资源池，分配内存
  - 若没有指定大小的空闲内存则申请新的 slab
- 输入：
  - size: 内存大小
- 输出：

```C
void* free_in_slab(void *addr){
  // 将该addr所在slot加入对应slab的free_list
}
```

- 功能: 释放从 slab 分配器中获取的指定块
