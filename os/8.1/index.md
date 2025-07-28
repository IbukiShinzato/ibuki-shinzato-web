---
title: OS8.1
---

### ./malloc_test -m 10000000 -l 10
![1.png](https://ie.u-ryukyu.ac.jp/~e235718/os/8.1/img/1.png)

### ./malloc_test -m 1000000 -l 10
![2.png](https://ie.u-ryukyu.ac.jp/~e235718/os/8.1/img/2.png)

### ./malloc_test -m 100000 -l 10
![3.png](https://ie.u-ryukyu.ac.jp/~e235718/os/8.1/img/3.png)

### ./malloc_test -m 10000 -l 10
![4.png](https://ie.u-ryukyu.ac.jp/~e235718/os/8.1/img/4.png)

### ./malloc_test -m 1000 -l 10
![5.png](https://ie.u-ryukyu.ac.jp/~e235718/os/8.1/img/5.png)

### ./malloc_test -c 800 -m 10000000 -l 10
![6.png](https://ie.u-ryukyu.ac.jp/~e235718/os/8.1/img/6.png)

### ./malloc_test -c 80 -m 10000000 -l 10
![7.png](https://ie.u-ryukyu.ac.jp/~e235718/os/8.1/img/7.png)


## 考察
これらの結果からメモリの要求する際にサイズのばらつきが大きいほど断片化のサイズが大きくなる。またメモリを要求する回数が多ければ多いほど利用しているメモリのサイズが小さくなる。逆にメモリを要求を少なくすると一度にプロセスに割り当てられるメモリのサイズが大きくなる。
