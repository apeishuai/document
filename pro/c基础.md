# 核心概念
- 重用机制、代码组织方式
	- 作用域：文件作用域、变量作用域
	- 宏、include头文件、函数
- 内存管理：
	- 堆、栈、全局变量区、代码区
	- malloc、realloc、xxx
	- 指针
- 变量生命周期：
	- 栈区auto初始化、堆区手动管理、static 全局变量
- 构建过程抽象方式：
	- 递归、函数指针、函数作为传递参数

# 作用域、变量初始化
函数作用域、代码块作用域、文件作用域、原型作用域

属于文件作用域，缺省状态下为external链接属性
static将external改为internal

静态变量：默认为0
自动变量：随机
	堆：手动管理
	栈：自动管理
函数d在缺省情况下具有external链接属性

```c
#include <stdio.h>
#include <stdlib.h>

//测试变量的生命周期
//结论：变量在执行完该函数后内存被销毁
void test_func_var(){
	//int func_var;
	static int func_var_static = 20; //作用域是函数内部
	int func_var = 6;
	printf("func_var value:%d\n",func_var);
	printf("addr b:%p\n",&func_var);
}


//测试参数地址
//结论：是一份拷贝，地址在变化
//var b的数值没有被改变
void test_param_constant(int b){
	b = 22;
	printf("var constant value:%d\n",b);
	printf("var constant addr (in func test_param):%p\n",&b);
}

//看能否改变内存数值
//可以改变内存数据 (数组在申明时会自动分配内存)
void test_param_arr(int arr[]){
	arr[4] = 6;
}

//看能否改变内存数据
//可以更改，需要手动赋值
void test_param_pointer(int *p){
	*p = 200;
}

int main(){
	//变量初始化方法
	//貌似数组申请内存个数跟我给的数字无关
	//int arr[] = {1,2,3};  //不计数量
	int arr[6] = {0};    //不计数量
	//int var_constant = 12;
	
	//int *p = (int *)malloc(sizeof(int));
	//* p = 20;

	test_func_var();
	//test_param_constant(var_constant);
	//printf("var constant addr (in func main):%p\n",&var_constant);
	//test_param_arr(arr);
	//test_param_pointer(p);

	/*for(int i = 0;i<6;i++){
		printf("test_param arr[]:%d\n",*(arr+i));
		printf("test_param arr[] addr:%p\n",arr+i);
	}*/

	printf("func_var_static:%d\n",func_var_static);

	return 0;
}


```
测试结构体
```c
typedef struct tag{ 
	struct tag *next;
	int xxx;
 }var_list;
```
```c
#include <stdio.h>
#include <stdlib.h>

#define LENGTH 200

typedef struct tag{
	struct tag *abc;
	int x;
	int y;
}var_struct;

void test_var_struct(){
	var_struct *pos = (var_struct *)malloc(sizeof(var_struct));
	pos->x = 100;
	pos->y = 200;

	printf("the coordiate of man is: \nx:%d\ny:%d\n",pos->x,pos->y);
}

typedef struct link_tag{
	struct link_tag *next;
	int data;
}link;


void test_link(link *ptr){
	link *ptr = (link *)malloc(LENGTH * sizeof(link));
	ptr->data = 200;
	ptr->next = NULL;
}


int main(){
	//test_var_struct();

	return 0;
}

```
# 函数
```c
return_type func_name(parameter list)
{
	body of the fun
}
param: formal real
```
参数：（按值传递 | 按地址传递）\
传递给函数的是一份指针的拷贝，但是对指针执行间接访问操作所访问的对象是原先的数组\
(ps:c没有按引用传递 即: func(&a))

(ps:函数无效，因为实际交换的是参数的拷贝)
```c
void swap(int x,int y)
{
int tmp;

tmp = x;
x = y;
y = tmp;
}
```
(ps:函数有效，实际交换的是变量指针的拷贝)
```c
void swap(int *x,int *y)
{
int tmp;

tmp = *x;
*x = *y;
*y = tmp;
}
```
# 字符串

# 其他
间接访问
*
[]
除了优先级外，下标引用和间接访问完全相同
2[array] = array[2]，神奇吧

stdlib.h
malloc() calloc() realloc() free

string.h
memcpy() memmove() memset() memcmp()


