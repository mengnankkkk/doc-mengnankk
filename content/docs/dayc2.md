---
title: C语言期末复习2
categories:
  - C
  
abbrlink: 2701
date: 2023.12.4
tags: 
   - C

---

## C语言常见代码汇总

## 判断一个年份是闰年

```c
int isleapyear(int year){
if ((year%4==0&&year%100!=0)||year%400==0){
return 1;
}
else{
return 0;
}
}
```

## 计算两个数的最大公约数（欧几里得算法）

```c
int gcd(int a,int b){
if (b==0){
return a;
}
return gcd(b,a%b);//递归计算最大公约数
}
```

## 二进制数转换为十进制数

```c
int binarytodecimal(int binary){
while(binary>0){
int reminder=binary%10;//获取最后一位数字
decimal+=reminder*base;//将最后一位数字乘以对应权重加到结果中
binary/=10;//去掉最后一位数字
base*=2;//每次权重乘以2
}
return decimal;//返回对应的十进制数
}
```

## 斐波那契数列（递归）

```c
int fibonacci(int n){
if (n<=1){
return n;//斐波那契数列的前两个数是1
}
return fibonacci(n-1)+fibonacci(n-2);//递归调用计算下一个数字
}
```

## 将一个十进制转化为二进制

```c
void decimaltobinary(int decimal){
int innary[32],index=0;
while(decimal>0){
binary[index++]=decimal%2
decimal/=2;
}
for (int i-=index-);i>=;i--){
printf("%d
,binary[i];
}
printf("\n");
}
```

## 求一个数组中的最大值和最小值

```c
void findminmax(int arr[],int size){
int min = arr[0],max = arr[0];
for (int i=1;i<size;i++){
if (arr[]<min){
min = arr[i];
}
if (arr[i]>max){
max=arr[i];
}
}
}
```

## 实现简单的计算器（+-x/）

```c
#include <stdio.h>
int add (int a,int b){
return a+b;
}
int subtract(int a,int b){
return a-b;
}
int multiply(int a,int b){
return a*b;
}
int divide(int a,int b){
if (b!=0){
return(float)a/b;
}
return -1;
}
int main(){
char expr[100];
int mun1,num2;
char op;
printf("请输入表达式");
scanf("%s",expr);
scanf(expr"%d %c %d",&num1,&op,&num2);
switch(op){
case '+':
printf("%d+%d=%d\n",num1,num2,add(num1,num2));
break;
case '-':
printf("%d-d=%d\n",num1,num2,subtract(num1,num2));
break;
case '*':
printf("%d*%d=%d\n"num1,num2, multiply(num1,num2));
break;
case '/':
printf("%d/%d=%.2f\n",num1,num2,divide(num1,num2);
break;
default:
printf("非法运算符\n);
break;

}
return 0;
}
```

## 冒泡排序学生总成绩

```c
#include <stdio.h>
#define MAX_N 100
struct Student{
	char name[20];
	char id[10];
	float ma,ch,total;
} stu[MAX_N];

void main(){
  int n=4,i,j;
  printf("请输入学生人数n=\n");
  scanf("%d",&n);
  printf("请输入学生信息:\n");
	for(i=0;i<n;i++){
		scanf("%s %s %f %f", &stu[i].name,&stu[i].id,&stu[i].ma,&stu[i].ch);
		stu[i].total=(stu[i].ma+stu[i].ch)/2;
	}
	for(i=0;i<n-1;i++){
		for(j=0;j<n-1-i;j++){
			if(stu[j].total>stu[j+1].total){
				struct Student mid;
				mid=stu[j];
				stu[j]=stu[j+1];
				stu[j+1]=mid;
			}
		}
	}
	for(i=0;i<n;i++) printf("%s %s %.2f\n", stu[i].name,stu[i].id,stu[i].total);
}

```

