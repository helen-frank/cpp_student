> # C语言的设计精神是：相信每个C程序员都是高手，不要阻止程序员去干他们需要干的事

# 复习C

## 剪刀石头布的精髓

```C
restult = (man - computer + 4) % 3 - 1;
```

完整代码

```C
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
int main(void)
{
    char gesture[3][10] = {"scissor", "stone", "cloth"};
    int man, computer, result, ret;
    srand(time(NULL));
    while (1)
    {
        computer = rand() % 3;
        printf("\nInput your gesture (0-scissor 1-stone 2-cloth):\n");
        ret = scanf("%d", &man);
        if (ret != 1 || man < 0 || man > 2)
        {
            printf("Invalid input! Please input 0, 1 or 2.\n");
            continue;
        }
        printf("Your gesture: %s\tComputer's gesture: %s\n", gesture[man], gesture[computer]);
        result = (man - computer + 4) % 3 - 1;
        if (result > 0)
            printf("You win!\n");
        else if (result == 0)
            printf("Draw!\n");
        else
            printf("You lose!\n");
    }
    return 0;
}

```

## 较真随机数生成

```c
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
int main()
{
    time_t times;
    srand((unsigned int)time(&times));//取随机数种子
    int l = rand();
    printf("%d",l);
}
```

## linux代码格式化工具indent

使用方法:

```shell
$ indent -kr -i8 main.c
```

$-kr$ 选项表示 $K\&R$ 风格，$-i8$ 表示缩进 8 个空格的长度。如果没有指定$-nut$选项，则每 8 个缩进空格会自动用一个$Tab$代替



## 排序算法

### 插入排序

反复执行该算法的for循环体，执行LEN-1次，就一定能把数组a排好序，而不管数组a的原始数据是什么

时间复杂度  O($n^2$)

```c
void insertion_sort(int a[], int len)
{
    int i, j, key;
    for (j = 1; j < len; ++j)
    {
        key = a[j];
        i = j - 1;
        while (i >= 0 && a[i] > key)
        {
            a[i + 1] = a[i];
            --i;
        }
        a[i + 1] = key;
    }
}
```

### 归并排序

采取分而治之（Divide-and-Conquer）的策略

时间复杂度 O($nlg^n$)

归并排序的步骤如下：

1. Divide: 把长度为n的输入序列分成两个长度为n/2的子序列。
2. Conquer: 对这两个子序列分别采用归并排序。
3. Combine: 将两个排序好的子序列合并成一个最终的排序序列。

```c

```

## 查找算法

### 折半查找

```c
int binarysearch(int number,int a[],int len)
{
    int mid, start = 0, end = len - 1;
    while (start <= end)
    {
        mid = (start + end) / 2;
        if (a[mid] < number)
            start = mid + 1;
        else if (a[mid] > number)
            end = mid - 1;
        else
            return mid;
    }
    return -1;
}
```

## 栈和队列

### 堆栈->深度优先搜索（DFS）

每次取一个相邻的点走下去，一直走到无路可走了再退回来，取另一个相邻的点再走下去。这称为深度优先搜索（DFS，Depth First Search）

#### 解决迷宫问题

```c
int maze[5][5] = { 
    0, 1, 0, 0, 0,
    0, 1, 0, 1, 0,
    0, 0, 0, 0, 0,
    0, 1, 1, 1, 0,
    0, 0, 0, 1, 0,
};
```

它表示一个迷宫，其中的1表示墙壁，0表示可以走的路，只能横着走或竖着走，不能斜着走，要求编程序**堆栈解决**找出从左上角到右下角的路线

```c

```

### 队列->广度优先搜索

广度优先是一种步步为营的策略，每次都从各个方向探索一步，将前线推进一步，队列中的元素总是由前线的点组成的，可见正是因为队列先进先出的性质使这个算法具有了广度优先的特点

广度优先搜索还有一个特点是可以找到从起点到终点的**最短路径**

#### 解决迷宫问题

```c
int maze[5][5] = { 
    0, 1, 0, 0, 0,
    0, 1, 0, 1, 0,
    0, 0, 0, 0, 0,
    0, 1, 1, 1, 0,
    0, 0, 0, 1, 0,
};
```

它表示一个迷宫，其中的1表示墙壁，0表示可以走的路，只能横着走或竖着走，不能斜着走，要求编程序**队列解决**找出从左上角到右下角的路线

```c

```

### 环形队列

将上面的队列改为环形队列，复用空间