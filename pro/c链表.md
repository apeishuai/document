date: 2023-12-20T15:34:30+08:00

# 注意点

+ malloc会申请随机地址，所以init函数要操作指针的指针，达到修改指针地址的目的
+ typedef末尾的是var-list，名字不一样，但是表示的是同一结构，这种命名更好帮助组织程序
+ c函数是按值传递，即拷贝一份传入参数；如果传入为指针地址，也能达到修改地址内容的目标

# Link.h
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
# Link.c
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
