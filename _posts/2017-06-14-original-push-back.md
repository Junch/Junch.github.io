---
layout: post
title: "vector分析(1) - 低效的push_back"
summary: "想不到vector有那么多坑，push_back算是第一个"
description: ""
tags: ["c++"]
categories: ["Debug"]
---

push_back的效率惊人的低下，插入100个元素居然要分配(Reallocate)内存**13次**。

 push_back elements #  | 1 | 2 | 3 | 4 | 5 | 7 | 10 | 14 | 20 | 29 | 43 | 64 | 95
-----------------------|---|---|---|---|---|---|----|----|----|----|----|----|---
 Reallocate capacity # | 1 | 2 | 3 | 4 | 6 | 9 | 13 | 19 | 28 | 42 | 63 | 94 | 141 

<br>

#### 1.测试代码

```cpp
TEST(VECTOR, push_alloc)
{
using Container = vector<A>;
    Container container;
    for (int  i=0; i< 100; ++i){
        container.push_back(A("item"));
    }
}
```

#### 2.调试步骤

在命令行中执行如下命令
```bash
windbg test.exe --gtest_filter=VECTOR.push_alloc
```

在windbg的命令中执行

```txt
0:000> r? $t0=0
0:000> x  test!std::vector<A,std::allocator<A> >::_Reallocate
00e9dfc0          test!std::vector<A,std::allocator<A> >::_Reallocate (unsigned int)
0:000> bp 00e9dfc0  ".printf \"count=%d num=%d\\n\", poi(@esp+4), @$t0;gc"
0:000> x test!std::vector<A,std::allocator<A> >::push_back
00e9f1d0          test!std::vector<A,std::allocator<A> >::push_back (class A *)
0:000> bp 00e9f1d0 "r $t0=@$t0+1;gc"
0:000> g
count=1 num=1
count=2 num=2
count=3 num=3
count=4 num=4
count=6 num=5
count=9 num=7
count=13 num=10
count=19 num=14
count=28 num=20
count=42 num=29
count=63 num=43
count=94 num=64
count=141 num=95
```

#### 3.解决方案
调用vector::reserve()

