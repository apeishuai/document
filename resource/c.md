# 前置知识

## 核心概念
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

## 基础语法
### 作用域、变量初始化
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
### 函数
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
#include <stdio.h>

void increment(int num)
{
	num++;
}

int main(){
	int x = 5;
	increment(x);
	printf("the num x`s value: %d",x);
	return 0;
}

```

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

按址传递
```c
void increment(int *num){
	(*num)++;
}

int main(){
	int x = 5;
	increment(&x);
	return 0;
}
```
按引用传递
```c
void increment(int &num){
	num++;
}

int main()
{
	int x = 5;
	increment(x);
	return 0;
}
```
传递数组
```c
void printarray(int arr[],int len){
	for(int i = 0;i<len;i++){
	printf("%d",arr[i]);
}
}

int main(){
	int arr[] = {1,2,3,4,5};
	int len = sizeof(arr)/sizeof(arr[0]);
	printarray(arr,len);
	return 0;
}
```

### 字符串

### 其他
间接访问\
*\
[]:访问数组型元素\
除了优先级外，下标引用和间接访问完全相同\
2[array] = array[2]，神奇吧

stdlib.h\
malloc() calloc() realloc() free\

string.h\
memcpy() memmove() memset() memcmp()\

date: 2023-12-11 09:55:30


# 数据结构
写顺序表遇到如下卡点：
+ c基础不到位(结构体、指针怎么使用，涉及到很多细节，补充c基础内容)
+ 功能层面：函数是否完备，仅需要内存管理、增删改查就够了吗?
+ 性能层面：想不到具体场景，日后完善

## Seq.h
```c
#include <stdio.h>
#include <stdlib.h>

#define CAP 100
#define EType int

#define status int
#define OK 1
#define ERROR 0

typedef struct{
  EType *elem;
  int length,maxsize;
}SqList;


#if 0
/*--------------------------------
            创建、销毁列表(内存操作)
 -------------------------------*/
status InitSeqList(SqList *L); 
status Expand(SqList *L); //扩展内存
status ClearSeqList(SqList *L); 
status DestorySeqList(SqList *L); 


/*--------------------------------
            增删改查
 -------------------------------*/
status InsertSeqList(SqList *L,int pos,EType e);
status DeleteElem(SqList *L,int pos);
status ModifyElem(SqList *L,EType e);
status ModifyElem(SqList *L,int pos,EType e);
status SearchData(SqList *L,EType data);
status SearchPos(SqList *L,int pos);

/*--------------------------------
            非破坏性操作
 -------------------------------*/
status ListEmpty(SqList *L);
status PrintElem(SqList *L);


/*--------------------------------
            复杂操作
 -------------------------------*/
合并两张表成一张
分解一张表成两张
#endif
```

## Seq.c
```c
#include "include/SqList.h"

status InitSeqList(SqList *L){
  L->elem = (EType *)malloc(CAP * sizeof(EType));
  if(!L->elem)
    return ERROR;
  L->length = 0;

#if 0 //测试PrintElem函数
  L->length = 4;
  L->elem[0] = 20;
  L->elem[1] = 20;
  L->elem[2] = 20;
  L->elem[3] = 60;
#endif
  
  L->maxsize = CAP;
  return OK;
}

status Expand(SqList *L){
  L->elem = (EType *)realloc(L->elem,(L->length + CAP) * sizeof(EType));
  if(!L->elem)
    return ERROR;
  L->maxsize += CAP;
  return OK;
}

status ClearSeqList(SqList *L){
  if(L->elem == NULL)
    return OK;
  free(L->elem);
  L->length = 0;
  //free(L)
  return OK;
}

status DestorySeqList(SqList *L){
  ClearSeqList(L);
  free(L);
  return OK;
}

status PrintElem(SqList *L){
  if(L->length == 0)
    printf("no elem");
  for(int i = 0;i < L->length;i++){
    //printf("%d\n",L->elem[i]); 下标访问
    printf("%d\n",*(((*L).elem /* = L->elem*/)+i));  //指针访问
  }
  return OK;
}

status InsertSeqList(SqList *L,int pos,EType e){
#if 1 // 0 隐藏代码；1 加入编译
  if(pos > L->maxsize){
    Expand(L);
    printf("expand storage\n");
    }
#endif
  
  if(pos<1 || pos > L->maxsize || pos > L->length+1)
    return ERROR;

  #if 0
  if(L->length == 0){
    *(L->elem) = e;
    L->length++;
    return OK;
  }
  #endif

  for(int i = L->length;i>=pos;i--)
    *((L->elem)+i) = *((L->elem)+i-1);
  *((L->elem)+pos-1) = e;
  L->length++;
  return OK;
}

status SearchData(SqList *L,EType data){
  for(int i = 0;i<L->length;i++){
    if(data == *((L->elem)+i))
      return OK;
  }
  return ERROR;
}

status DeleteElem(SqList *L,int pos){   //删除第i个元素
  if(pos<1 || pos > L->length)
    return ERROR;
  for(int i = pos-1;i<L->length;i++){
    *((L->elem)+i) = *((L->elem)+i+1);
  }
  L->length--;
  return OK;
}

status ListEmpty(SqList *L){
  if(L->length == 0)
    printf("Seq is empty");
  else
    printf("Seq is not empty");
  return OK;
};

status ModifyElemPos(SqList *L,int pos,EType e){
  if(pos<1 || pos > L->length)
    return ERROR;
  *((L->elem)+pos-1) = e;
  return OK;
}

main()
{
  SqList *ptr;
  InitSeqList(ptr);

  #if 0 
  if(InitSeqList(ptr))
  printf("ok\n");
  #endif

  InsertSeqList(ptr,1,10);
  InsertSeqList(ptr,2,15);
  InsertSeqList(ptr,2,20);
  InsertSeqList(ptr,2,30);
  ModifyElemPos(ptr,2,100);
  PrintElem(ptr);

  #if 0
  if(SearchData(ptr,30))
    printf("have found");
  else
    printf("no elem");
  #endif

  #if 0 //test del func
  printf("before:\n");
  PrintElem(ptr);
  printf("after delete:\n");
  DeleteElem(ptr,4);
  #endif

  #if 0
  if(Expand(ptr))
    {
    printf("ass finished\n");
    printf("length: %d\n",ptr->length);
    printf("maxsize: %d\n",ptr->maxsize);
    }
  #endif
  
  #if 0 //测试destory后ptr能否继续使用
  if(DestorySeqList(ptr)){
      printf("destoryed\n");
    }
  InsertSeqList(ptr,1,10);
  PrintElem(ptr);
  #endif
}

```
date: 2023-12-20T15:34:30+08:00

注意点
+ malloc会申请随机地址，所以init函数要操作指针的指针，达到修改指针地址的目的
+ typedef末尾的是var-list，名字不一样，但是表示的是同一结构，这种命名更好帮助组织程序
+ c函数是按值传递，即拷贝一份传入参数；如果传入为指针地址，也能达到修改地址内容的目标

## Link.h
```c
#include <stdio.h>
#include <stdlib.h>


#define EType int

#define status int
#define OK 1
#define ERROR 1

typedef struct LinkNode{
	EType data;
	struct LinkNode *next;
}Node,Head,Link;
```
## Link.c
```c
#include "include/Link.h"

void Init(Head **ptr){
  *ptr = (Head *)malloc(sizeof(Head));
  (*ptr)->data = -1;
  (*ptr)->next = NULL;
}

void Insert(Link *L,EType e){
  Node *tmp = (Node *)malloc(sizeof(Node));
  tmp->data = e;
  tmp->next = NULL;

  while(L->next)
    L = L->next;
  L->next = tmp;
}

status PrintElem(Link *L){
  while(L->next != NULL){
    L = L->next;
    printf("%d\n",L->data);
  }
  return OK;
}

main(){
  Head *ptr = NULL;
  Init(&ptr);
  Insert(ptr,2);
  Insert(ptr,4);
  PrintElem(ptr);
}
```
## stack
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 10

typedef struct{
	int data[MAX_SIZE];
	int top;
}stack;

void init(stack *s){
	s->top = -1;
}

bool isempty(stack *s){
	return s->top == -1;
}

bool isfull(stack *s){
	return s->top == MAX_SIZE - 1; 
}

bool push(stack *s,int value){
	if(isfull(s)){
		printf("stack is full\n");
		return false;
	}
	s->data[++s->top] = value;
	return true;
}

int pop(stack *s){
	if(isempty(s)){
		printf("stack is empty\n");
		exit(EXIT_FAILURE);
	}
	return s->data[s->top--];
}

int peek(stack *s){
	if(isempty(s)){
		printf("stack is empty\n");
		exit(EXIT_FAILURE);
	}
	return s->data[s->top];
}

int main()
{
	stack s;
	init(&s);
	push(&s,10);
	push(&s,20);
	push(&s,30);
	push(&s,40);
	printf("the top element is %d\n",peek(&s));

	int x;
	while(!isempty(&s)){
		x = pop(&s);
		printf("%d\n",x);
	}

	return 0;
}
```
## queue
```c
#include <stdio.h>
#include <stdlib.h>
#include <stdbool.h>

#define MAX_SIZE 10

typedef struct{
	int data[MAX_SIZE];
	int front;
	int rear;
}queue;

void init(queue *q){
	q->front = 0;
	q->rear = 0;
}

bool isempty(queue *q){
	return q->front == q->rear;
}

bool isfull(queue *q){
	return ((q->rear + 1)%MAX_SIZE) == q->front; 
}

bool enqueue(queue *q,int value){
	if(isfull(q)){
		printf("queue is full\n");
		return false;
	}
	q->data[q->rear] = value;
	q->rear = (q->rear + 1 )% MAX_SIZE;
	return true;
}

int dequeue(queue *q){
	if(isempty(q)){
		printf("queue is empty\n");
		exit(EXIT_FAILURE);
	}
	int value = q->data[q->front];
	q->front = (q->front + 1)%MAX_SIZE;
	return value;
}

int peek(queue *q){
	if(isempty(q)){
		printf("stack is empty\n");
		exit(EXIT_FAILURE);
	}
	return q->data[q->front];
}

int main()
{
	queue q;
	init(&q);
	enqueue(&q,10);
	enqueue(&q,20);
	enqueue(&q,30);
	printf("the top element is %d\n",peek(&q));

	int x;
	while(!isempty(&q)){
		x = dequeue(&q);
		printf("%d\n",x);
	}

	return 0;
}
```
## 二叉树
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct reeNode{
	int value;
	struct reeNode *left;
	struct reeNode *right;
}TreeNode;

TreeNode* create(int value){
	TreeNode* newNode = (TreeNode*)malloc(sizeof(TreeNode));
	if(!newNode){
		printf("memory allocation failed\n");
		return NULL;
	}
	newNode->value = value;
	newNode->left = NULL;
	newNode->right = NULL;
	return newNode;
}


void preorderTraversal(TreeNode* root){
	if(root == NULL) return;
	printf("%d\n",root->value);
	preorderTraversal(root->left);
	preorderTraversal(root->right);
}

void midorderTraversal(TreeNode* root){
	if(root == NULL) return;
	midorderTraversal(root->left);
	printf("%d\n",root->value);
	midorderTraversal(root->right);
}


void backorderTraversal(TreeNode* root){
	if(root == NULL) return;
	backorderTraversal(root->left);
	backorderTraversal(root->right);
	printf("%d\n",root->value);
}

int main()
{
	TreeNode* root = create(1);
	root->left = create(2);
	root->right = create(3);
	root->left->left = create(4);
	root->left->right = create(5);

	printf("前序遍历:\n");
	preorderTraversal(root);
	
	printf("中序遍历:\n");
	midorderTraversal(root);

	printf("后序遍历:\n");
	backorderTraversal(root);
	
	    // 释放分配的内存（在实际应用中，应该递归释放所有节点）
    	free(root->left->left);
    	free(root->left->right);
    	free(root->left);
    	free(root->right);
    	free(root);
}
```
## 二叉搜索树
```c
#include <stdio.h>
#include <stdlib.h>

typedef struct node{
	int data;
	struct node *left;
	struct node *right;
}Node;

typedef struct{
	Node* root;
}Tree;


void insert(Tree* tree,int value)
{
	Node *node = malloc(sizeof(Node));
	node->data = value;
	node->left = NULL;
	node->right=NULL;
	if(tree->root == NULL){
		tree->root = node;
	}
	else{
		Node *tmp = tree->root;
		while(tmp != NULL){
			if(value < tmp->data){
				if(tmp->left == NULL){
					tmp->left = node;
					return ;
				}
				else{
					tmp = tmp->left;
				}
			}
			else{
				if(tmp->right==NULL){
					tmp->right=node;
					return ;
				}
				else{
					tmp=tmp->right;
				}
			}
		}
	}


}


int get_height(Node* node){
	if(node == NULL)
		return 0;
	else{
		int left_h = get_height(node->left);
		int right_h = get_height(node->right);
		int max = left_h;
		if(right_h > max){
			max = right_h;
		}
		return max+1;
	}
}

int get_maxnum(Node* node){
	if(node == NULL)
		return -1;
	else{
		int m1 = get_maxnum(node->left);
		int m2 = get_maxnum(node->right);
		int m3 = node->data;
		int max = m1;
		if(m2>max){max = m2;}
		if(m3>max){max = m3;}
		return max;
	}

}

void preorder(Node* node){
	if(node != NULL){
		printf("%d\n",node->data);
		preorder(node->left);
		preorder(node->right);
	}
}

int main()
{
	int arr[7] = {6,3,8,2,5,1,7};
	Tree tree;
	tree.root = NULL;

	for(int i = 0;i<7;i++){
		insert(&tree,arr[i]);
	}

	//preorder(tree.root);
	int h = get_height(tree.root);
	printf("the height of tree is %d\n",h);

	int m = get_maxnum(tree.root);
	printf("the max num is: %d",m);
	
	return 0;
}

```

# 算法
排序-冒泡排序
```c
```



# c snipaste
猜数字游戏
```c
#include <stdio.h>
#include <time.h>
#include <stdlib.h>


int main()
{
	int guess;
	int i = 1;
	int real = rand()%101;

	while(i < 10){
		printf("please input the guess num: ");
		scanf("%d",&guess);
		i++;

	if(guess > real)
		printf("bigger than real;\n");
	else if(guess < real)
		printf("smaller than real;\n");
	else
		printf("success\n");
	}
	return 0;
}
 
```
