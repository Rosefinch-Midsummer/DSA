# MOOC-PTA排序题目

<!-- toc -->

## PTA 09-排序1 排序

略

## PTA 09-排序2 Insert or Merge

According to Wikipedia:

**Insertion sort** iterates, consuming one input element each repetition, and growing a sorted output list. Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain.

**Merge sort** works as follows: Divide the unsorted list into N sublists, each containing 1 element (a list of 1 element is considered sorted). Then repeatedly merge two adjacent sublists to produce new sorted sublists until there is only 1 sublist remaining.

Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, can you tell which sorting method we are using?

Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤100). Then in the next line, N integers are given as the initial sequence. The last line contains the partially sorted sequence of the N numbers. It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.

Output Specification:

For each test case, print in the first line either "Insertion Sort" or "Merge Sort" to indicate the method used to obtain the partial result. Then run this method for one more iteration and output in the second line the resuling sequence. It is guaranteed that the answer is unique for each test case. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

Sample Input 1:

```
10
3 1 2 8 7 5 9 4 6 0
1 2 3 7 8 5 9 4 6 0
```

Sample Output 1:

```
Insertion Sort
1 2 3 5 7 8 9 4 6 0
```

Sample Input 2:

```
10
3 1 2 8 7 5 9 4 0 6
1 3 2 8 5 7 4 9 0 6
```

Sample Output 2:

```
Merge Sort
1 2 3 8 4 5 7 9 0 6
```


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202312081616167.png)


![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202312081616510.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202312081616119.png)

![](https://cdn.jsdelivr.net/gh/Rosefinch-Midsummer/MyImagesHost03/img/202312081617928.png)



```c
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

int Compare_Array(ElementType A[], ElementType B[], int N)
{
    int flag = 1;
    for(int i=0; i<N; i++)
    {
        if(A[i] != B[i])
        {
            flag = 0;
            break;
        }
    }
    return flag;
}



void Merge(ElementType A[], ElementType TmpA[], int L, int R, int RightEnd)
{
    int LeftEnd, NumElements, Tmp;
    int i;
    LeftEnd = R - 1;
    Tmp = L;
    NumElements = RightEnd - L + 1;

    while(L <= LeftEnd && R <= RightEnd)
    {
        if(A[L] < A[R])
            TmpA[Tmp++] = A[L++];
        else
            TmpA[Tmp++] = A[R++];
    }
    while(L <= LeftEnd)
        TmpA[Tmp++] = A[L++];
    while(R <= RightEnd)
        TmpA[Tmp++] = A[R++];
    for(i=0; i<NumElements; i++, RightEnd--)
        A[RightEnd] = TmpA[RightEnd];
}

void MSort(ElementType A[], ElementType TmpA[], int L, int RightEnd)
{
    int Center;

    if(L < RightEnd)
    {
        Center = (L + RightEnd)/2;
        MSort(A, TmpA, L, Center);
        MSort(A, TmpA, Center+1, RightEnd);
        Merge(A, TmpA, L, Center+1, RightEnd);
    }
}


void Merge_pass( ElementType A[], ElementType TmpA[], int N, int length )
{ /* 两两归并相邻有序子列 */
     int i, j;

     for ( i=0; i <= N-2*length; i += 2*length )
         Merge( A, TmpA, i, i+length, i+2*length-1 );
     if ( i+length < N ) /* 归并最后2个子列*/
         Merge( A, TmpA, i, i+length, N-1);
     else /* 最后只剩1个子列*/
         for ( j = i; j < N; j++ ) TmpA[j] = A[j];
}

void PrintA(ElementType A[], int N)
{
    int First = 1;
    for(int i=0; i<N; i++)
    {
        if(First)
        {
            printf("%d", A[i]);
            First = 0;
        }
        else
            printf(" %d", A[i]);
    }
}



void Merge_Or_Insertion( ElementType A[], ElementType Temp[], int N )
{
     int length;
     ElementType *TmpA;
     int flag = 0;
     length = 1; /* 初始化子序列长度*/
     TmpA = malloc( N * sizeof( ElementType ) );
     if ( TmpA != NULL ) {
          while( length < N ) {
              Merge_pass( A, TmpA, N, length );
              length *= 2;
              if(flag == 1) {
                    PrintA(A,N);
                    flag = 0;
                }
                if(Compare_Array(A,Temp,N)) {
                    flag = 1;
                    printf("Merge Sort\n");
                }
              Merge_pass( TmpA, A, N, length );
              length *= 2;
              if(flag == 1) {
                    PrintA(A,N);
                    flag = 0;
                }
                if(Compare_Array(A,Temp,N)) {
                    flag = 1;
                    printf("Merge Sort\n");
                }
          }
          free( TmpA );
     }
     else printf( "空间不足" );
}


void CopyArr(int A[],int B[],int N)
{
    int i;
    for(i=0;i<N;i++)
        B[i] = A[i];
}

void Insertion_Or_Merge(ElementType A[], ElementType B[], int N)
{
    int flag = 0;
    for(int P=1; P<N; P++)
    {
        ElementType temp = A[P];
        int i;
        for(i=P; i>0&&A[i-1]>temp; i--)
            A[i] = A[i-1];
        A[i] = temp;
        if(Compare_Array(A,B,N))
        {
            printf("Insertion Sort\n");
            flag = 1;
            continue;
        }
        if(flag)
        {
            PrintA(A,N);
            return;
        }
    }
    return;
}

int main()
{
    int N;
    scanf("%d", &N);
    ElementType A[N];
    for(int i=0; i<N; i++)
    {
        scanf("%d", &A[i]);
    }
    ElementType Temp[N];
    for(int i=0; i<N; i++)
    {
        scanf("%d", &Temp[i]);
    }
    ElementType *A2 = (ElementType*)malloc(N*sizeof(ElementType));
    ElementType *Temp2 = (ElementType*)malloc(N*sizeof(ElementType));
    CopyArr(A,A2,N);
    CopyArr(Temp,Temp2,N);
    Insertion_Or_Merge(A,Temp,N);
    Merge_Or_Insertion(A2, Temp2, N);

    return 0;
}
```


## PTA 09-排序3 Insertion or Heap Sort

According to Wikipedia:

**Insertion sort** iterates, consuming one input element each repetition, and growing a sorted output list. Each iteration, insertion sort removes one element from the input data, finds the location it belongs within the sorted list, and inserts it there. It repeats until no input elements remain.

**Heap sort** divides its input into a sorted and an unsorted region, and it iteratively shrinks the unsorted region by extracting the largest element and moving that to the sorted region. it involves the use of a heap data structure rather than a linear-time search to find the maximum.

Now given the initial sequence of integers, together with a sequence which is a result of several iterations of some sorting method, can you tell which sorting method we are using?

Input Specification:

Each input file contains one test case. For each case, the first line gives a positive integer N (≤100). Then in the next line, N integers are given as the initial sequence. The last line contains the partially sorted sequence of the N numbers. It is assumed that the target sequence is always ascending. All the numbers in a line are separated by a space.

Output Specification:

For each test case, print in the first line either "Insertion Sort" or "Heap Sort" to indicate the method used to obtain the partial result. Then run this method for one more iteration and output in the second line the resulting sequence. It is guaranteed that the answer is unique for each test case. All the numbers in a line must be separated by a space, and there must be no extra space at the end of the line.

Sample Input 1:

```
10
3 1 2 8 7 5 9 4 6 0
1 2 3 7 8 5 9 4 6 0
```

Sample Output 1:

```
Insertion Sort
1 2 3 5 7 8 9 4 6 0
```

Sample Input 2:

```
10
3 1 2 8 7 5 9 4 6 0
6 4 5 1 0 3 2 7 8 9
```

Sample Output 2:

```
Heap Sort
5 4 3 1 0 2 6 7 8 9
```


要点：增加标志，控制输出时机

```c
#include <stdio.h>
#include <stdlib.h>

typedef int ElementType;

void Swap(ElementType *a, ElementType *b)
{
    ElementType temp = *a;
    *a = *b;
    *b = temp;
}

void PercDown(ElementType A[], int p, int N)
{
    int Parent,Child;
    ElementType X;
    X = A[p];

    for(Parent=p; (Parent*2+1)<N; Parent=Child)
    {
        Child = Parent*2+1;
        if(Child!=N-1 && A[Child]<A[Child+1])
            Child++;
        if(X>=A[Child])
            break;
        else
            A[Parent] = A[Child];
    }
    A[Parent] = X;
}

void Heap_Sort(ElementType A[], int N)
{
    for(int i=N/2-1; i>=0; i--)
    {
        PercDown(A,i,N);
    }
    for(int i=N-1; i>0; i--)
    {
        Swap(&A[0],&A[i]);
        PercDown(A, 0, i);
    }
}


int Compare_Array(ElementType A[], ElementType B[], int N)
{
    int flag = 1;
    for(int i=0; i<N; i++)
    {
        if(A[i] != B[i])
        {
            flag = 0;
            break;
        }
    }
    return flag;
}

void PrintA(ElementType A[], int N)
{
    int First = 1;
    for(int i=0; i<N; i++)
    {
        if(First)
        {
            printf("%d", A[i]);
            First = 0;
        }
        else
            printf(" %d", A[i]);
    }
}

void CopyArr(int A[],int B[],int N)
{
    int i;
    for(i=0;i<N;i++)
        B[i] = A[i];
}

void Insertion_Or_Merge(ElementType A[], ElementType B[], int N)
{
    int flag = 0;
    for(int P=1; P<N; P++)
    {
        ElementType temp = A[P];
        int i;
        for(i=P; i>0&&A[i-1]>temp; i--)
            A[i] = A[i-1];
        A[i] = temp;
        if(Compare_Array(A,B,N))
        {
            printf("Insertion Sort\n");
            flag = 1;
            continue;
        }
        if(flag)
        {
            PrintA(A,N);
            return;
        }
    }
    return;
}

void Heap_Or_Insertion(ElementType A[], ElementType B[], int N)
{
    for(int i=N/2-1; i>=0; i--)
    {
        PercDown(A,i,N);
    }
    int flag = 0;
    for(int i=N-1; i>0; i--)
    {
        Swap(&A[0],&A[i]);
        PercDown(A, 0, i);
        if(Compare_Array(A,B,N))
        {
            printf("Heap Sort\n");
            flag = 1;
            continue;
        }
        if(flag)
        {
            PrintA(A,N);
            return;
        }
    }
}

int main()
{
    int N;
    scanf("%d", &N);
    ElementType A[N];
    for(int i=0; i<N; i++)
    {
        scanf("%d", &A[i]);
    }
    ElementType Temp[N];
    for(int i=0; i<N; i++)
    {
        scanf("%d", &Temp[i]);
    }
    ElementType *A2 = (ElementType*)malloc(N*sizeof(ElementType));
    ElementType *Temp2 = (ElementType*)malloc(N*sizeof(ElementType));
    CopyArr(A,A2,N);
    CopyArr(Temp,Temp2,N);
    Insertion_Or_Merge(A,Temp,N);
    Heap_Or_Insertion(A2, Temp2, N);

    return 0;
}
```




## PTA 10-排序4 统计工龄

给定公司N名员工的工龄，要求按工龄增序输出每个工龄段有多少员工。

输入格式:

输入首先给出正整数N（≤105），即员工总人数；随后给出N个整数，即每个员工的工龄，范围在`[0, 50]`。

输出格式:

按工龄的递增顺序输出每个工龄的员工个数，格式为：“工龄:人数”。每项占一行。如果人数为0则不输出该项。

输入样例:

```
8
10 2 0 5 7 2 5 2
```

输出样例:

```
0:1
2:3
5:2
7:1
10:1
```

```c
#include <stdio.h>

typedef int ElementType;

int main()
{
    int N;
    scanf("%d", &N);
    ElementType Result[51]={0};
    ElementType temp;
    for(int i=0; i<N; i++)
    {
        scanf("%d", &temp);
        Result[temp]++;
    }
    for(int i=0; i<51; i++)
    {
        if(Result[i]==0)
                continue;
        else
            printf("%d:%d\n",i,Result[i]);
    }
    return 0;
}
```

## PTA 10-排序5 PAT Judge

The ranklist of PAT is generated from the status list, which shows the scores of the submissions. This time you are supposed to generate the ranklist for PAT.

Input Specification:

Each input file contains one test case. For each case, the first line contains 3 positive integers, N (≤104), the total number of users, K (≤5), the total number of problems, and M (≤105), the total number of submissions. It is then assumed that the user id's are 5-digit numbers from 00001 to N, and the problem id's are from 1 to K. The next line contains K positive integers `p[i]` (`i`=1, ..., K), where `p[i]` corresponds to the full mark of the i-th problem. Then M lines follow, each gives the information of a submission in the following format:

```
user_id problem_id partial_score_obtained
```

where `partial_score_obtained` is either −1 if the submission cannot even pass the compiler, or is an integer in the range `[0, p[problem_id]]`. All the numbers in a line are separated by a space.

Output Specification:

For each test case, you are supposed to output the ranklist in the following format:

```
rank user_id total_score s[1] ... s[K]
```

where `rank` is calculated according to the `total_score`, and all the users with the same `total_score` obtain the same `rank`; and `s[i]` is the partial score obtained for the `i`-th problem. If a user has never submitted a solution for a problem, then "-" must be printed at the corresponding position. If a user has submitted several solutions to solve one problem, then the highest score will be counted.

The ranklist must be printed in non-decreasing order of the ranks. For those who have the same rank, users must be sorted in nonincreasing order according to the number of perfectly solved problems. And if there is still a tie, then they must be printed in increasing order of their id's. For those who has never submitted any solution that can pass the compiler, or has never submitted any solution, they must NOT be shown on the ranklist. It is guaranteed that at least one user can be shown on the ranklist.

Sample Input:

```
7 4 20
20 25 25 30
00002 2 12
00007 4 17
00005 1 19
00007 2 25
00005 1 20
00002 2 2
00005 1 15
00001 1 18
00004 3 25
00002 2 25
00005 3 22
00006 4 -1
00001 2 18
00002 1 20
00004 1 15
00002 4 18
00001 3 4
00001 4 2
00005 2 -1
00004 2 0
```

Sample Output:

```out
1 00002 63 20 25 - 18
2 00005 42 20 0 22 -
2 00007 42 - 25 - 17
2 00001 42 18 18 4 2
5 00004 40 15 0 25 -
```

```c



```

## PTA 10-排序6 Sort with Swap(0, i)

Given any permutation of the numbers {0, 1, 2,..., N−1}, it is easy to sort them in increasing order. But what if `Swap(0, *)` is the ONLY operation that is allowed to use? For example, to sort {4, 0, 2, 1, 3} we may apply the swap operations in the following way:

```
Swap(0, 1) => {4, 1, 2, 0, 3}
Swap(0, 3) => {4, 1, 2, 3, 0}
Swap(0, 4) => {0, 1, 2, 3, 4}
```

Now you are asked to find the minimum number of swaps need to sort the given permutation of the first N nonnegative integers.

Input Specification:

Each input file contains one test case, which gives a positive N (≤105) followed by a permutation sequence of {0, 1, ..., N−1}. All the numbers in a line are separated by a space.

Output Specification:

For each case, simply print in a line the minimum number of swaps need to sort the given permutation.

Sample Input:

```
10
3 5 7 2 6 4 9 0 8 1
```

Sample Output:

```
9
```

```c

```









