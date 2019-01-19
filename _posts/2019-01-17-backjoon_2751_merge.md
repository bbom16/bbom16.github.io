---
layout: post
title: "알고리즘 문제풀이 - 백준 2751(merge sort를 이용하여 구현)"
tags: [알고리즘, 백준]
comments: true

---

# **백준 2751 < 수 정렬하기2 >**

### 1. 문제
N개의 수가 주어졌을 때, 이를 오름차순으로 정렬하는 프로그램을 작성하시오.

### 2. 문제 이해
1. 문제는 간단하지만 시간 제한 때문에 bubble sort과 insertion sort와 같은 O(n)인 정렬을 이용하면 시간 초과가 난다. 수행 시간이 O(nlogn)인 merge sort와 heap sort를 이용하여 문제를 해결해야 한다.

### 3. 문제 해결
**Merge sort**를 이용하여 해결한다.
#### **알고리즘**

|  0   |  1   |  2   |  3   |  4   |
| :--: | :--: | :--: | :--: | :--: |
|  5   |  4   |  3   |  2   |  1   |

위와 같은 배열을 내림차순으로 정렬한다고 가정한다.  

- divide 과정 : 처음엔 2개의 배열로 나누는 과정을 거친다. index를 반으로 쪼개면서 본다.  
  - 1 단계 : 5 4 3 과 2 1
  - 2 단계 : 5 4 / 3 / 2 1
  - 3 단계 : 5 / 4 / 3 / 2 / 1  
  모두 다 쪼개지면 다음 단계로 진행한다.
- conquer 와 merge 단계 : 부분 배열을 정렬하고, 정렬된 배열을 합친다.
  sorted라는 별도의 공간을 둬서 정렬된 값들을 저장하고, 다시 원래의 공간으로 복사해준다. 정렬할 때, 두 부분 배열의 처음 index들을 비교해서 더 작은 값을 sorted 배열에 저장해준다.
  - 1 단계 : 5 / 4 -> 4 5 , 2 / 1 -> 1 2
  - 2 단계 :  4 5 / 3 -> 3 4 5, 1 2
  - 3 단계 : 3 4 5 / 1 2 -> 1 2 3 4 5  
  -> 두 배열의 처음 index인 1과 3을 비교한다. 둘 중 더 작은 1이 sorted에 들어가고, 그 다음 2와 3을 비교한다. 더 작은 2가 sorted에 들어간다. 이런 식으로 배열을 정렬하게 된다.

#### **알고리즘 구현**

- divide 과정
  - 재귀를 이용하여 index를 반으로 쪼개가며 2개의 배열로 나눠줬다.

```c
void Merge_sort(int left, int right)
{
    int mid;
    if(left<right){
        mid = (left + right)/2;
        Merge_sort(left, mid);
        Merge_sort(mid+1, right);
        merge(left, mid, right);
    }
}
```
- conquer & merge 단계
  - 두 배열의 첫 인덱스들을 비교하여 더 작은 값을 sorted 배열에 넣어주면서 두 배열의 비교를 반복해서 정렬했다.

```c
void merge(int left, int mid, int right)
{
    int i,j,k;
    i = left;   //첫번째 리스트 0
    j = mid+1;  //2번째 리스트 0
    k = left;  //sorted 배열 index

    // sorted 배열로 저장
    while(1){
        if(num[i]<= num[j]) sorted[k++] = num[i++];
        else sorted[k++] = num[j++];
        if(i == mid + 1){
            for(int s=j; s<right+1; s++)
                sorted[k++] = num[s];
            break;
        }
        else if(j == right + 1){
            for(int s=i; s<mid+1; s++)
                sorted[k++] = num[s];
            break;
        }
    }

    /* sorted 배열에 저장한 값 다시 num 배열로 저장*/
    for(i=left; i<right+1; i++)
        num[i] = sorted[i];
}
```

- 전체코드

```c
#include <stdio.h>

int N;
int num[1000001];
int sorted[1000001];

/* merge sort */
void Merge_sort(int left, int right);
void merge(int left, int mid, int right);
void swap(int* a, int* b);

int main(int argc, char * argv[])
{
    scanf("%d", &N);

    /* merge sort*/
     for(int i=0; i<N; i++){
     scanf("%d", &num[i]);
     }
     Merge_sort(0, N-1);
     for(int i=0; i<N; i++){
     printf("%d\n", num[i]);
     }

    return 0;
}

/* swap 함수 */
void swap(int* a, int* b)
{
    int tmp = *a;
    *a = *b;
    *b = tmp;
}


/* conquer & merge*/
void merge(int left, int mid, int right)
{
    int i,j,k;
    i = left;   //첫번째 리스트 0
    j = mid+1;  //2번째 리스트 0
    k = left;  //sorted 배열 index

    /* sorted 배열로 저장*/
    while(1){
        if(num[i]<= num[j]) sorted[k++] = num[i++];
        else sorted[k++] = num[j++];
        if(i == mid + 1){
            for(int s=j; s<right+1; s++)
                sorted[k++] = num[s];
            break;
        }
        else if(j == right + 1){
            for(int s=i; s<mid+1; s++)
                sorted[k++] = num[s];
            break;
        }
    }

    /* sorted 배열에 저장한 값 다시 num 배열로 저장*/
    for(i=left; i<right+1; i++)
        num[i] = sorted[i];
}

/* divide 과정*/
void Merge_sort(int left, int right)
{
    int mid;
    if(left<right){
        mid = (left + right)/2;
        Merge_sort(left, mid);
        Merge_sort(mid+1, right);
        merge(left, mid, right);
    }
}
```

문제 : [https://www.acmicpc.net/problem/2751](https://www.acmicpc.net/problem/2751)