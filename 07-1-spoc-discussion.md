## 课堂思考题

```
Bakery没有choosing的情况

考虑两个进程p0和p1，pid p0 < pid p1
初始情况下时num[0]和num[1]都为0
p0计算完max后，在赋值之前切换到进程1执行
P1计算完并赋值得到num[1] = 1
此时由于num[0] = 0，所以p1进入临界区执行
然后p0进行赋值得到num[0] = 1，pid p0 < pid p1
p0也会进入临界区执行
```

## SPOC思考题

#### 2.（spoc)了解race condition. 进入race-condition代码目录。

* 执行 ./x86.py -p loop.s -t 1 -i 100 -R dx， 请问dx的值是什么？

`-1`

* 执行 ./x86.py -p loop.s -t 2 -i 100 -a dx=3,dx=3 -R dx ， 请问dx的值是什么？

`-1`

* 执行 ./x86.py -p loop.s -t 2 -i 3 -r -a dx=3,dx=3 -R dx， 请问dx的值是什么？

`-1`

* 变量x的内存地址为2000, ./x86.py -p looping-race-nolock.s -t 1 -M 2000, 请问变量x的值是什么？

`1`

* 变量x的内存地址为2000, ./x86.py -p looping-race-nolock.s -t 2 -a bx=3 -M 2000, 请问变量x的值是什么？为何每个线程要循环3次？

`6`

* 变量x的内存地址为2000, ./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 0， 请问变量x的值是什么？

`2`

* 变量x的内存地址为2000, ./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 1， 请问变量x的值是什么？

`1或2`

* 变量x的内存地址为2000, ./x86.py -p looping-race-nolock.s -t 2 -M 2000 -i 4 -r -s 2， 请问变量x的值是什么？

`1或2`

* 变量x的内存地址为2000, ./x86.py -p looping-race-nolock.s -a bx=1 -t 2 -M 2000 -i 1， 请问变量x的值是什么？

`-1`
