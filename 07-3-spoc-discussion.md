##（扩展练习）请在lab7-answer中分析

* cvp->count含义是什么？cvp->count是否可能<0, 是否可能>1？请举例或说明原因。

```
1.cvp->count含义是等待条件变量的进程数量；
2.不可能<0，因为cvp->count的增加和减少是成对出现的；
3.可能>1，此时有多个进程处于等待。
```

* cvp->owner->next_count含义是什么？cvp->owner->next_count是否可能<0, 是否可能>1？请举例或说明原因。

```
1.cvp->owner->next_count含义是发出条件变量的信号正在等待的进程数目；
2.不可能<0，因为cvp->owner->next_count的增加都是在减少之前进行的；
3.不可能>1，因为cvp->owner->next_count的增加和减少是成对出现的。
```

* 目前的lab7-answer中管程的实现是Hansen管程类型还是Hoare管程类型？请在lab7-answer中实现另外一种类型的管程。

```
Hoare管程类型
```
