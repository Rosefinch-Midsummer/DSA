# MOOC-PTA复杂度题目

<!-- toc -->

## PTA 01-复杂度1 最大子列和问题

给定K个整数组成的序列{ N1​, N2​, ..., NK​ }，“连续子列”被定义为{ Ni​, Ni+1​, ..., Nj​ }，其中 1≤i≤j≤K。“最大子列和”则被定义为所有连续子列元素的和中最大者。例如给定序列{ -2, 11, -4, 13, -5, -2 }，其连续子列{ 11, -4, 13 }有最大的和20。现要求你编写程序，计算给定整数序列的最大子列和。

本题旨在测试各种不同的算法在各种数据情况下的表现。各组测试数据特点如下：

- 数据1：与样例等价，测试基本正确性；
- 数据2：102个随机整数；
- 数据3：103个随机整数；
- 数据4：104个随机整数；
- 数据5：105个随机整数；

输入格式:

输入第1行给出正整数K (≤100000)；第2行给出K个整数，其间以空格分隔。

输出格式:

在一行中输出最大子列和。如果序列中所有整数皆为负数，则输出0。

输入样例:

```in
6
-2 11 -4 13 -5 -2
```

输出样例:

```out
20
```

### 三重循环求解


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310081535329.png)

```c
#include <stdio.h>

int MaxSubSeq(int A[], int N)
{
    int ThisSum = 0,MaxSum = 0;
    for(int i=0;i < N;i++)
    {
        for(int j = i;j < N;j++)
        {
            ThisSum = 0;
            for(int k=i;k<=j;k++)
                ThisSum += A[k];
            if(ThisSum>MaxSum)
                MaxSum = ThisSum;
        }
    }
    return MaxSum;
}

int main()
{
    int arr[100];
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        scanf("%d",&arr[i]);
    printf("%d",MaxSubSeq(arr,n));
    return 0;
}
```

### 二重循环求解

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310081535616.png)

```c
#include <stdio.h>

int MaxSubSeq(int A[], int N)
{
    int ThisSum = 0,MaxSum = 0;
    for(int i=0;i < N;i++)
    {
        ThisSum = 0;
        for(int j = i;j < N;j++)
        {
            ThisSum += A[j];
            if(ThisSum>MaxSum)
                MaxSum = ThisSum;
        }
    }
    return MaxSum;
}

int main()
{
    int arr[100];
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        scanf("%d",&arr[i]);
    printf("%d",MaxSubSeq(arr,n));
    return 0;
}
```

### 分治算法

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310081546769.png)


```c
#include <stdio.h>

int Max3( int A, int B, int C )
{ /* 返回3个整数中的最大值 */
    return A > B ? A > C ? A : C : B > C ? B : C;
}

int DivideAndConquer( int List[], int left, int right )
{ /* 分治法求List[left]到List[right]的最大子列和 */
    int MaxLeftSum, MaxRightSum; /* 存放左右子问题的解 */
    int MaxLeftBorderSum, MaxRightBorderSum; /*存放跨分界线的结果*/

    int LeftBorderSum, RightBorderSum;
    int center, i;

    if( left == right )  { /* 递归的终止条件，子列只有1个数字 */
        if( List[left] > 0 )  return List[left];
        else return 0;
    }

    /* 下面是"分"的过程 */
    center = ( left + right ) / 2; /* 找到中分点 */
    /* 递归求得两边子列的最大和 */
    MaxLeftSum = DivideAndConquer( List, left, center );
    MaxRightSum = DivideAndConquer( List, center+1, right );

    /* 下面求跨分界线的最大子列和 */
    MaxLeftBorderSum = 0; LeftBorderSum = 0;
    for( i=center; i>=left; i-- ) { /* 从中线向左扫描 */
        LeftBorderSum += List[i];
        if( LeftBorderSum > MaxLeftBorderSum )
            MaxLeftBorderSum = LeftBorderSum;
    } /* 左边扫描结束 */

    MaxRightBorderSum = 0; RightBorderSum = 0;
    for( i=center+1; i<=right; i++ ) { /* 从中线向右扫描 */
        RightBorderSum += List[i];
        if( RightBorderSum > MaxRightBorderSum )
            MaxRightBorderSum = RightBorderSum;
    } /* 右边扫描结束 */

    /* 下面返回"治"的结果 */
    return Max3( MaxLeftSum, MaxRightSum, MaxLeftBorderSum + MaxRightBorderSum );
}

int MaxSubseqSum3( int List[], int N )
{ /* 保持与前2种算法相同的函数接口 */
    return DivideAndConquer( List, 0, N-1 );
}

int main()
{
    int arr[100];
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        scanf("%d",&arr[i]);
    printf("%d",MaxSubseqSum3(arr,n));
    return 0;
}
```

### 在线处理（动态规划）

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost01/img/202310081549404.png)

```c
#include <stdio.h>

int MaxSubSeq(int A[], int N)
{
    int i;
    int ThisSum,MaxSum;
    ThisSum = 0,MaxSum = 0;
    for(i=0;i < N;i++)
    {
        ThisSum += A[i];
        if(ThisSum > MaxSum)
            MaxSum = ThisSum;
        else if(ThisSum < 0)
            ThisSum = 0;
    }
    return MaxSum;
}

int main()
{
    int arr[100];
    int n;
    scanf("%d",&n);
    for(int i=0;i<n;i++)
        scanf("%d",&arr[i]);
    printf("%d",MaxSubSeq(arr,n));
    return 0;
}
```



## PTA 01-复杂度2 Maximum Subsequence Sum

Given a sequence of K integers { N1​, N2​, ..., NK​ }. A continuous subsequence is defined to be { Ni​, Ni+1​, ..., Nj​ } where 1≤i≤j≤K. The Maximum Subsequence is the continuous subsequence which has the largest sum of its elements. For example, given sequence { -2, 11, -4, 13, -5, -2 }, its maximum subsequence is { 11, -4, 13 } with the largest sum being 20.

Now you are supposed to find the largest sum, together with the first and the last numbers of the maximum subsequence.

Input Specification:

Each input file contains one test case. Each case occupies two lines. The first line contains a positive integer K (≤10000). The second line contains K numbers, separated by a space.

Output Specification:

For each test case, output in one line the largest sum, together with the first and the last numbers of the maximum subsequence. The numbers must be separated by one space, but there must be no extra space at the end of a line. In case that the maximum subsequence is not unique, output the one with the smallest indices i and j (as shown by the sample case). If all the K numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence.

Sample Input:

```in
10
-10 1 2 3 4 -5 -23 3 7 -21
```

Sample Output:

```out
10 1 4
```

注意：这里需特别注意输出部分，因为题目中说“_If all the K numbers are negative, then its maximum sum is defined to be 0, and you are supposed to output the first and the last numbers of the whole sequence._”（当输入的这K个数全为负时，输出的最大子列和应为0，并输出整个输入序列的首尾两个数）。那就需要对应MaxSum >= 0和MaxSum < 0的情况，之所以要把 = 0的情况放到前者，是因为 = 0可能是输入序列中有0有负的结果，比如输入6个数-2，0，-1，0，0，0。那最大子列和也是0但是显然不应输出题意要求的全为负情况下的首末两个数（-2和0）而是输出正常比较后的0和0（对应下标都为1）。

这也涉及到和前一题不同的MaxSum初值问题，因为输入全为负的情况下，累加时ThisSum不断重置为0，MaxSum保持不变，最后直接进入输出的else情况，符合题意。

```
6
-2 0 -1 0 0 0
ThisSum=0, MaxSum=-1, MaxIndex=0, MinIndex=0, tmpMinIndex=1
ThisSum=0, MaxSum=0, MaxIndex=1, MinIndex=1, tmpMinIndex=1
ThisSum=0, MaxSum=0, MaxIndex=1, MinIndex=1, tmpMinIndex=3
ThisSum=0, MaxSum=0, MaxIndex=1, MinIndex=1, tmpMinIndex=3
ThisSum=0, MaxSum=0, MaxIndex=1, MinIndex=1, tmpMinIndex=3
ThisSum=0, MaxSum=0, MaxIndex=1, MinIndex=1, tmpMinIndex=3
0 0 0
```

```c
#include <stdio.h>

void MaxSubSumWithIndex(int List[], int K)
{
    int i;
    int MaxIndex = 0, MinIndex = 0, tmpMinIndex = 0;
    int ThisSum = 0, MaxSum = -1;    //这里MaxSum不能改成0
    for(i = 0; i < K; i++)
    {
        ThisSum += List[i];
        if (ThisSum > MaxSum)
        {
            MaxSum = ThisSum;
            MaxIndex = i;
            MinIndex = tmpMinIndex;
        }else if(ThisSum < 0)
        {
            ThisSum = 0;
            tmpMinIndex = i + 1;
        }
        printf("ThisSum=%d, MaxSum=%d, MaxIndex=%d, MinIndex=%d, tmpMinIndex=%d\n",ThisSum, MaxSum, MaxIndex, MinIndex, tmpMinIndex);
    }
    if(MaxSum >= 0)
      printf("%d %d %d\n", MaxSum, List[MinIndex], List[MaxIndex]);
    else
      printf("0 %d %d\n", List[0], List[K - 1]);
}

int main()
{
    int n;
    scanf("%d",&n);
    int arr[n];
    for(int i = 0; i<n; i++)
        scanf("%d",&arr[i]);
    MaxSubSumWithIndex(arr, n);
    return 0;
}
```

## PTA 01-复杂度3 二分查找

本题要求实现二分查找算法。

函数接口定义：

```c++
Position BinarySearch( List L, ElementType X );
```

其中`List`结构定义如下：

```c
typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};
```

`L`是用户传入的一个线性表，其中`ElementType`元素可以通过>、`==`、<进行比较，并且题目保证传入的数据是递增有序的。函数`BinarySearch`要查找`X`在`Data`中的位置，即数组下标（注意：元素从下标1开始存储）。找到则返回下标，否则返回一个特殊的失败标记`NotFound`。

裁判测试程序样例：

```c
#include <stdio.h>
#include <stdlib.h>

#define MAXSIZE 10
#define NotFound 0
typedef int ElementType;

typedef int Position;
typedef struct LNode *List;
struct LNode {
    ElementType Data[MAXSIZE];
    Position Last; /* 保存线性表中最后一个元素的位置 */
};

List ReadInput(); /* 裁判实现，细节不表。元素从下标1开始存储 */
Position BinarySearch( List L, ElementType X );

int main()
{
    List L;
    ElementType X;
    Position P;

    L = ReadInput();
    scanf("%d", &X);
    P = BinarySearch( L, X );
    printf("%d\n", P);

    return 0;
}

/* 你的代码将被嵌在这里 */
```

\输入样例1：

```
5
12 31 55 89 101
31
```

输出样例1：

```
2
```

输入样例2：

```
3
26 78 233
31
```

输出样例2：

```
0
```

```c
Position BinarySearch( List L, ElementType X )
{
    Position left = 1;
    Position right = L->Last;
    Position mid;
    while(left <= right)
    {
        mid = (left + right) / 2;
        if(L->Data[mid] < X)
            left = mid + 1;
        else if(L->Data[mid] > X)
            right = mid - 1;
        else
            return mid;
    }
    return NotFound;
}
```









