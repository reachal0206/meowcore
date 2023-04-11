# Module: memory

功能：内存管理模块；

通常 M 模式的程序在第一次进入 S 模式之前会把零写入 satp 以禁用分页，然后 S 模式的程序在初始化页表以后会再次进行 satp 寄存器的写操作。

- `satp`寄存器：RV64 中为 64 位读写寄存器，用于控制 S-Mode 下的地址翻译和保护
  ![satp寄存器最大长度为64](/docs/pics/satp%20XLEN=64.jpg)
  - PPN[43:0]:保存了一级页表的物理页号 PPN（物理地址/4KiB）；
  - ASID[59:44]: 由 hart 唯一所有，用于标识不同地址空间；
  - MODE[63:60]: 用于选择地址翻译机制；
    - 0: Bare 没有翻译和保护
    - 8: Sv39
    - 9：Sv48
      ![Sv48](/docs/imgs/Sv48.jpg)
    - 10: Sv57
    - 11: Sv64(Reversed)

## x. Module

### x.1 buddy

### x.2 slub

## 1.Struct

### 1.x physical_page

```C
typedef struct physical_page{
  // int allocated;
  // void* slab;

}physical_page;
```

4k 物理页

### 1.x vmspace

```C
typedef struct vmspace{
  virt_addr pgtbl_addr; //页表基地址
  list vmregions; // 虚拟内存区域链表

}vmspace;
```

虚拟地址空间

### 1.x vmregion

```C
typedef struct vmregion{
  struct list_head head; // 虚拟内存区域链表头
  vaddr_t start;  // 起始虚拟地址
  size_t size;  // 内存区域大小
  vmr_prop_t perm;  //  权限
  struct pmobject *pmo;  //

}vmregion;
```

### 1.x pmobject

```C
typedef struct pmobject{
  paddr *start; // 起始地址
  size_t size; // 地址大小
  pmo_prop_t perm; // 权限
  // 类型
}pmobject;
```

物理内存对象

## 2.Functions

### 2.1 memory_init

```C
void memory_init(long syscall_id, long *args)
```

- 功能：初始化内存管理模块
  - 初始化各 CPU 页表，开启页表映射
- 输入：
  - `syscall_id`：系统调用号
  - `args`：参数列表
- 输出：该系统调用的返回值

### 2.x malloc

```C
void malloc(size_t size)
```

- 功能：分配指定大小的内存
- 输入：
  - `size`：内存区域大小
- 输出：
  - 内存区域指针？

### 2.x free

```C
void free(void *ptr)
```

- 功能：释放内存
- 输入：
  - ptr: 待释放内存起始地址
- 输出：

### 2.x get_pages(int order)

```C
void get_pages(int order)
```

- 功能：获取 $2^{order}$ 个 4k 物理页
- 输入：
  - order: 连续物理页大小
- 输出：
  - 连续物理页起始页指针（起始地址）

### 2.x free_pages(void \*ptr)

```C
void free_pages(void *ptr)
```

- 功能：释放指定物理页
- 输入：
  - ptr: 待释放物理页起始地址

### 2.x vmspace_init

```C
void vmspace_init(struct vmspace *vmspace)
```

- 功能：初始化虚拟地址空间
  - 分配该 vmspace 下的一级页表
- 输入：
  - vmspace: 待初始化

### 2.x vmspace_init

```C
void vmspace_init(struct vmspace *vmspace)
```

- 功能：初始化虚拟地址空间
  - 分配该 vmspace 下的一级页表
- 输入：
  - vmspace: 待初始化

### 2.x add_vmr_to_vmspace

```C
void add_vmr_to_vmspace(struct vmspace *vmspace, struct vmregion *vmregion)
```

- 功能：向 vmspace 中添加 vmregion
- 输入：
  - vmspace:
  - vmregion

### 2.x del_vmr_from_vmspace

```C
void del_vmr_from_vmspace(struct vmspace *vmspace, struct vmregion *vmregion)
```

- 功能：从 vmspace 中删除 vmregion
- 输入：
  - vmspace:
  - vmregion

### 2.x fill_page_table

```C
void fill_page_table(struct vmspace *vmspace, struct vmregion *vmregion)
```

- 功能：填写指定 vmregion 的页表
- 输入：
  - vmspace:
  - vmregion

### 2.x map_range_in_pgtbl

```C
int map_range_in_pgtbl(struct vmspace *vmspace, vaddr_t va, paddr_t pa, size_t len, vmr_prot_t flags)
```

- 功能：填写 vmspace 内指定地址范围的页表
- 输入：
  - vmspace:
  - va: 起始虚拟地址
  - pa: 起始物理地址
  - len: 地址范围大小
  - flags:
- 输出：返回页表映射是否成功添加

### 2.x unmap_range_in_pgtbl

```C
int unmap_range_in_pgtbl(struct vmspace *vmspace, vaddr_t va, size_t len)
```

- 功能：删除页表映射
- 输入：
  - vmspace:
  - va: 起始虚拟地址
  - : 地址范围大小
- 输出：返回页表映射是否成功添加

### 2.x mmap

```C
long mmap(void *start, size_t len, int prot, int flags, int fd, off_t off)
```

- 功能：将文件或设备映射到内存中
- 输入：
  - start: 起始地址；
  - len: 长度；
  - prot: 内存保护方式；
    - PROT_EXEC；
    - PROT_READ；
    - PROT_WRITE；
    - PROT_NONE；
  - flags: 是否与其他进程共享的标志位；
  - fd: 文件句柄；
  - off: 文件偏移量
- 输出：成功则返回已映射区域的指针，失败则返回-1.

### 2.x munmap

```C
int munmap(void *start, size_t len)
```

- 功能：将文件或设备取消映射到内存中；
- 输入：

  - start: 起始地址；
  - len: 长度；

- 输出：成功返回 0，失败返回-1;

### 2.x brk

```C
uintptr_t brk(uintptr_t brk)
```

- 功能：修改数据段的大小；
- 输入：

  - brk: 指定待修改的地址；

- 输出：成功返回 0，失败返回-1;
