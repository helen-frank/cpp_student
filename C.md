> # C语言的设计精神是：相信每个C程序员都是高手，不要阻止程序员去干他们需要干的事

# 复习C

**剪刀石头布的精髓**

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

