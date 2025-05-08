---
title: C语言期末复习3
categories:
  - C
  
abbrlink: 2701
date: 2023.12.25
tags: 
   - C

---



# 某年某月某日

```c
#include <stdio.h>
void main()
{
    int year,month,day;
    scanf("%d %d",&year,&month);
    switch(month){
        case 1: case 3: case 5: case 7: case 8: case 10: case 12:
            day=31;
            break;
        case 4: case 6: case 9: case 11:
            day=30;
            break;
        case 2: day=(year%4==0&&year%100!=0||year%400==0)?29:28;
    }
    printf("%d年%d月有%d天",year,month,day);
}
```

# 判断素数（函数）

```c
int is_prime(int a)
{
    int i =0;
    if(a<=1){
        return -1;
    }
    for(i=2;i<=sqrt(a);i++){
        if(a%i==0){
            return 0;
        }

    }
    return 1;
}
```

# 冒泡排序

```c
void sortarr2(int a,int n){
    int i,j;
    for(i=0;j<n-1;i++){
        for(j=1;j<n-i;j++){
            if(a[j]<a[j-1]){
                int temp;
                temp=a[j-1];
                a[j-1]=a[j];
                a[j]=temp;
            }
        }
    }
    return;
}

```

# 选择排序

```c
void sortarry1(int *a,int n)
{
    int i,j;
    for(i=0;i<n-1;i++){
        for (j=i+1;j<n;j++){
            if(a[i]>a[j]){
                int temp;
                temp=a[i];
                a[i]=a[j];
                a[j]=temp;
            }
        }
    }
    return ;
}
```

# 二分查找

```c
int search(int* a,int key,int n)
{
    int first;
    int last=n-1;
    int mid=0;
    while(first<last)
    {
        mid=(first+last)/2;
        if(a[mid]>=key)
        {
            last=mid;
        }
        else 
        {
            first=mid+1;
        
        }
    }
    if(a[last]==key){
        return mid;
    }
    return -1
}
```

# 数组最大值平均数

```c
#include <stdio.h>
#define MAX_N 100
void main(){
    float nums[MAX_N];
    int n,i;
    float max,avg;
    printf("请输入数组个数n=");
    scanf("%d",&n);
    for (i=0;i<n;i++){
        scanf("%f",&nums[i]);
        avg+=nums[i];
    }   
    max=nums[0];
    for(i=0;i<n;i++) (max<nums[i])?(max=nums[i]):max;
    printf("max=%.2f,avg=%.2f",max,avg/n);
}
```

# 素数判断

```c
#include <stdio.h>
#include <math.h>
void main (){
    int i,j,isprinme=1;
    for(i=2;i<=100;i++){
        if (i==2||i==3) isprinme=1;
        else if (i%6==1&&i%6!=5) isprinme=0;
        else {
            for(j=5;j<sqrt(i);j+=6){
                if(i%j=0||i%(j+2)==0){
                    isprinme=0;
                    break;
                }
            }
        }
        
    }
    (isprinme==1)?printf("%d\t",i):(isprinme=1);
}
```

# 水仙花数

```c
#include <stdio.h>
#include <math.h>
void main() {
	int i;
	for (i=100;i<1000;i++) (i==pow(i/100,3)+pow(i%10,3)+pow(i/10%10,3))?printf("%d\t", i):0;
}

```

# 大小写转换

```c
#include <stdio.h>
#define Max_n 100
void main()
{
    char strs[Max_n];
    int i=0;
    printf("请输入字符串");
    gets(strs);
    while(strs[i]!='0'){
        if(strs[i]>=65&&strs[i]<=90) strs[i]+=32;
        printf("%c",strs[i++]);
    }
}
```

# 杨辉三角的输出

```c
#include <stdio.h>
int tri(int r,int c){
    return (c==1||c==r)?1:tri(r-1,c-1)+tri(r-1,c);
}
void main()
{
    int i,j,n;
    printf("请输入杨辉三角的行数（1~20");
    scanf("%d",&n);
    for(i=1;i<=n;i++){
        for(j=0;j<n-1;j++) printf("%c%c%c",32,32,32);
        for(j=1;j<=i;j++) printf("%-6d",tri(i,j));
        printf("\n");
    }

}
```

# 阶乘数的显示

```c
#include <stdio.h>
int kn(int n){
    return (n==0)?1:n*kn(n-1);
}
void main()
{
    int n,i;
    printf("请输入阶乘数n:");
    scanf("%d",&n);
    printf("%d!=",n);
    for(i=n;i>0;i--) (i!=1)?printf("%d*",i):printf("%d",i);
    printf("=%d",kn(n));
}
```

# 逆序输出数字

```c
#include <stdio.h>
void main()
{
    int n;
    scanf("%d",&n);
    while(n>0){
        (n%10!=0)?printf("%d",n%10):0;
        n/=10;
    }
}
```

