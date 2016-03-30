### 试分析ucore操作系统内核是如何把子进程exit()的返回值传递给父进程wait()的？

通过proc_struct中的exit_code变量来完成返回值传递。

在do_exit()函数中，子进程将返回值保存在自身结构体的exit_code变量中。

```
current->exit_code = error_code;
```

在do_wait()函数中，父进程通过访问子进程的exit_code变量取得子进程exit()的返回值。

```
*code_store = proc->exit_code;
```
