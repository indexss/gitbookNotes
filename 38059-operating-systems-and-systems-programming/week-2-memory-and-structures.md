# Week 2 - Memory & Structures

### Memory

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240330011532299.png" alt="" width="188"><figcaption></figcaption></figure>

Text段，包含程序，可执行的指令

两个Data段分别包含已初始化和未初始化的全局和静态变量

Stack段用于存储所有局部或自动变量。当我们向函数传递参数时，它们被保存在堆栈中

Heap段用于存储动态分配的变量。**注意，图只是演示，heap段不一定从data段后开始**。

栈从高地址到低地址存

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240330012211034.png" alt=""><figcaption></figcaption></figure>

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240330012303101.png" alt=""><figcaption></figcaption></figure>

***

Local Variable暂时在stack段中，生命周期为那个函数

static variable存在data段中，数据会被保存，作用域为那个自己的函数。生命周期为全部程序。

global变量存在数据段中，作用域为所有函数，生命周期为全部程序。

***

### Pass-by-reference and Pass-by-value

pass by reference就是将指针作为参数传入。

***

函数可以返回指针

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240331223220376.png" alt=""><figcaption></figcaption></figure>

***

陷阱：不要返回一个函数内部变量指针

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240331223621351.png" alt=""><figcaption></figcaption></figure>

***

### Dynamic Memory Management

stdlib.h中提供了动态内存申请的函数

malloc():

```
 int *p;
 // Block of memory is allocated
 if((p= (int *) malloc(3*sizeof(int)))==NULL){
   printf("Allocation failed");
   exit(-1);
 }
 // [Some statements involving p]
 // Block of memory pointed by p is released
 free(p);
```

1. 从Heap申请连续的内存空间
2. 返回第一个字节的申请到的空间
3. 如果申请失败，得到NULL指针

***

Memory leak：申请了，但没有free()ed。就是内存泄露。在C中，防止内存泄漏是你的责任

可以使用valgrind来检查是否有内存泄漏问题。

```
 valgrind --leak-check=full ./a.out
```

***

在现代操作系统中，应用使用的内存在应用结束时应该被释放

* 如果这个泄露内存的程序只跑短时间，那么没有严重问题
* 如果这个泄露内存的程序会跑很长时间，那么就会有问题

***

内存泄漏例子：

```
 int main(){
   int *p1,**p2;
   p1 = malloc(sizeof(int));
   *p1=7;
   p2 = malloc(sizeof(int*));
   *p2=p1;
   return 0;
 }
```

![image-20240331224745953](file:///Users/linlishi/Library/Application%20Support/typora-user-images/image-20240331224745953.png?lastModify=1711898913)

这样会泄露出去4+8

***

```
 int main(){
   int *p1,
   **p2;
   p1 = malloc(sizeof(int));
   *p1=7;
   p2 = malloc(sizeof(int*));
   *p2=p1;
   free(p1);
   return 0;
 }
```

这样会泄露8，因为p2指向的东西没有被释放

***

```
 int main(){
   int *p1,
   **p2;
   p1 = malloc(sizeof(int));
   *p1=7;
   p2 = malloc(sizeof(int*));
   *p2=p1;
   free(p2);
   return 0;
 }
```

这样会泄露4，因为7没有被释放，只有那个指向7的指针被释放了

***

```
 int main(){
   int *p1,
   **p2;
   p1 = malloc(sizeof(int));
   *p1=7;
   p2 = malloc(sizeof(int*));
   *p2=p1;
   free(p1);
   free(p2);
   return 0;
 }
```

***

不要对一个地址双重释放两次，不然会崩溃。因为在可用blocks中，会出现两个p1。

<figure><img src="https://cdn.jsdelivr.net/gh/indexss/imagehost@main/img/image-20240331232724419.png" alt=""><figcaption></figcaption></figure>
