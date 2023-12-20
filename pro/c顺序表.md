date: 2023-12-11T09:55:30+08:00

# 写顺序表遇到如下卡点：
+ c基础不到位(结构体、指针怎么使用，涉及到很多细节，补充c基础内容)
+ 功能层面：函数是否完备，仅需要内存管理、增删改查就够了吗?
+ 性能层面：想不到具体场景，日后完善

# Seq.h
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

# Seq.c
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
