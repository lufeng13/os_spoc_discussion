# lab4 spoc 思考题

- 有"spoc"标记的题是要求拿清华学分的同学要在实体课上完成，并按时提交到学生对应的ucore_code和os_exercises的git repo上。

## 个人思考题

### 13.1 总体介绍

(1) ucore的线程控制块数据结构是什么？

是 proc_struct。

``` cpp
struct proc_struct {
    enum proc_state state;                      // Process state
    int pid;                                    // Process ID
    int runs;                                   // the running times of Proces
    uintptr_t kstack;                           // Process kernel stack
    volatile bool need_resched;                 // bool value: need to be rescheduled to release CPU?
    struct proc_struct *parent;                 // the parent process
    struct mm_struct *mm;                       // Process's memory management field
    struct context context;                     // Switch here to run process
    struct trapframe *tf;                       // Trap frame for current interrupt
    uintptr_t cr3;                              // CR3 register: the base addr of Page Directroy Table(PDT)
    uint32_t flags;                             // Process flag
    char name[PROC_NAME_LEN + 1];               // Process name
    list_entry_t list_link;                     // Process link list 
    list_entry_t hash_link;                     // Process hash list
};
```

### 13.2 关键数据结构

(2) 如何知道ucore的两个线程同在一个进程？

查看这两个线程的cr3和mm_struct是否相同，相同则是同一个进程。

(3) context和trapframe分别在什么时候用到？

在进程切换的时候context和trapframe都要用到，在系统调用/中断/异常时会用到trapframe。

(4) 用户态或内核态下的中断处理有什么区别？在trapframe中有什么体现？

两者中断处理都要保存ErrorCode、EIP、CS、EFLAGS，在用户态中中断处理，trapframe中会多压入ss和esp用于栈的替换。

### 13.3 执行流程

(5) do_fork中的内核线程执行的第一条指令是什么？它是如何过渡到内核线程对应的函数的？
```
tf.tf_eip = (uint32_t) kernel_thread_entry;
/kern-ucore/arch/i386/init/entry.S
/kern/process/entry.S
```

第一条指令为“pushl %edx”。在kernel_thread函数中设置trapframe中的reg_rbx为函数入口地址,reg_edx为函数参数。在进程切换后进入entry.s执行：

第一条指令为`pushl %edx`。
do_fork调用kernel_thread函数设置trapframe中的reg_rbx为函数的入口地址，reg_edx为函数参数，最后切换至entry.s执行。
``` cpp
# entry.s
pushl %edx
call *%ebx
```

(6)内核线程的堆栈初始化在哪？

```
tf和context中的esp
```

初始化在setup_kstack函数内。

(7)fork()父子进程的返回值是不同的。这在源代码中的体现中哪？

在do_fork()函数中，父进程得到的返回值是子进程的pid，而把子进程的值设为0。

(8)内核线程initproc的第一次执行流程是什么样的？能跟踪出来吗？

> proc.c和sched.c中

```
   proc_init() initproc初始化
-> cpu_idle() idleproc让出CPU
-> schedule() 调度、切换到initproc
-> proc_run() initproc运行
-> switch_to() 进程切换
```
