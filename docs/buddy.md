# Module: buddy

伙伴系统模块

## Structs

### physical_page

```C

```

## Functions

### buddy_init

```C
void buddy_init(struct physical_page *start_page, long page_num)
```

- 功能:伙伴系统初始化
  - 初始化伙伴系统的各个物理页
  - 初始化各级空闲链表
- 输入
  - start_page: 起始物理页指针
  - page_num: 分配给伙伴系统的物理页个数

### buddy_alloc_pages

```C
struct physical_page* buddy_alloc_pages(int order)
```

- 功能: 从伙伴系统中分配 2^order 个连续 4k 物理页
  - 寻找空闲物理页链表
  - 分配页
- 输入
  - order: 需要的连续物理页大小
- 输出
  - 连续物理页（伙伴块）起始页指针

### buddy_free_pages

```C
void buddy_free_pages(struct physical_page *page)
```

- 功能: 释放伙伴块
  - 标记物理页为空闲
  - 合并伙伴块
- 输入
  - page: 伙伴块起始物理页
- 输出
