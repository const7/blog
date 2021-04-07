---
layout: post
title: 青蛙跳台阶变种
tags: [Algorithm, Programming]
description: 青蛙跳台阶变种问题
keywords: 算法, 青蛙, 台阶
mathjax: true
comment: true
---

一只青蛙一次可以跳上 1 级台阶，也可以跳上 2 级，但年幼的青蛙不能连续跳 2 级，求年幼青蛙跳上一个 n 级的台阶总共有多少种方法？

---

**目录**

* TOC
{:toc}

## 递归

假设 n 级台阶有 f(n) 种跳法，则有

$$f(n) =
\begin{cases}
f(n-1),  & \text{if 上一步跳了 2 级} \\
f(n-1)+f(n-2), & \text{if 上一步跳了 1 级}
\end{cases}$$

当上一步跳的是 1 级时，则当前这一步可跳 1 级或 2 级，否则当前只能跳 1 级，根据公式很容易写出递归代码：

```cpp
// n：剩余台阶级数
// two：为 1 表示上一步跳了 2 级，为 0 表示上一步跳了1级
int jump(int n, int two) {
    if (n <= 1) return n;
    if (n == 2) return two ? 1 : 2;
    return two ? (jump(n - 1, 0)) : (jump(n - 1, 0) + jump(n - 2, 1));
}
```

该解法比较容易理解，但显然递归解法效率是很低的，很容易想到利用动态规划来解决它。

## 动态规划

利用数组保存每次计算得到的值，避免重复计算。

主要思路与递归类似，只不过从递归计算变成了迭代的数组计算。这里给出自底向上的动态规划解法：

```cpp
int jump(int n) {
    vector<vector<int>> v(n + 1, vector<int>(2, 0));
    // v[i][0]表示上一跳为 1 级时，剩余 i 级台阶有 v[i][0] 种跳法
    // v[i][1]表示上一跳为 2 级时，剩余 i 级台阶有 v[i][1] 种跳法
    v[1][0] = 1;
    v[1][1] = 1;
    if (n >= 2) {
        v[2][0] = 2;
        v[2][1] = 1;
    }
    for (int i = 3; i <= n; ++i) {
        v[i][0] = v[i - 1][0] + v[i - 2][1];	
        v[i][1] = v[i - 1][0];
    }
    return v[n][0];
}
```

时间复杂度为 O(N)。

## 组合数学

把问题抽象成简单数学模型，假设有 x 次 2 级跳，y 次 1 级跳，则 x、y 必定满足如下关系式：

$$\begin{cases}
2x + y = n & \text{(1)} \\
x \le y+1 & \text{(2)}
\end{cases}$$

其中 (1) 式是显然的，而 (2) 式若不满足则必定存在相邻的 2 级跳。

要使 x 个 2 级跳均不相邻，可以使用插空法对所有组合数进行分析。

$$\underbrace{...\quad\underbrace{2\quad2\quad2\quad2}_{x 个 2}\quad...}_{x+1 个空位}$$

其中由于不能有相邻的 2，因此先给中间 $x-1$ 个空位插入 $x-1$ 个 1，然后将剩下 $n-2x-(x-1)=n-3x+1$ 个 1 任意分配到 $x+1$ 个空位。

问题就转化为了经典的 $a$ 个球放入 $b$ 个盒子的问题，$a$ 个球相同，$b$ 个盒子不同，盒子可以为空，组合数为 $C_{a+b-1}^{a-1}$。在本问题中 $a=n-3x+1$, $b=x+1$。代码如下：

```cpp
// 计算 C(a, b)
int cal_c(int a, int b) {
    if (a == 0)
        return 0;
    if (b == 0) return 1;

    int a_fac = 1, b_fac = 1;
    for (int i = a; i > a - b; --i)
        a_fac *= i;
    for (int i = 1; i <= b; ++i)
        b_fac *= i;
    return a_fac / b_fac;
}

int jump(int n) {
    if (n <= 2) return n;
    int total = 0;
    int a, b;
    for (int x = 0; x <= (n + 1) / 3; ++x) {
        a = n - 3 * x + 1;
        b = x + 1;
        total += cal_c(a + b - 1, b - 1);
    }
    return total;
}
```

时间复杂度 O(N)。
