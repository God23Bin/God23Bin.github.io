---
title: C语言-内存函数的实现（一）之memcpy
author: god23bin
tags:
  - C语言
  - 函数实现
categories: C
description: 关于memcpy函数的实现，就是...
abbrlink: f54dd169
date: 2021-04-20 16:21:23
aplayer:
---

C语言中的内存函数有如下这些

- memcpy
- memmove
- memcmp
- memset

## 下面看看memcpy函数

### memcpy

我们想想，之前有那个字符串拷贝的函数，即strcpy函数。都有拷贝的函数了，为什么还要这个内存拷贝函数呢？

不能直接用strcpy吗？这是一个好问题，那下面就试试它。

我们准备两个整型数组，分别为arr1和arr2，然后通过strcpy函数把arr1的内容拷贝到arr2中，代码如下

```c
int main() 
{
	int arr1[] = { 1,2,3,4,5 };
	int arr2[5] = { 0 };
	// 把arr1的内容拷贝到arr2中
	strcpy(arr2, arr1);
	return 0;
}
```

那么这里肯定会有警告，因为类型不同。

直接运行，通过Debug，看内存，可以发现实现不了完整的拷贝，strcpy只拷贝了一个字节

显然，strcpy函数不适用于其他类型的数据拷贝，**所以呢，就出现内存拷贝了，使任意类型的数据都能进行拷贝**。

老规矩，我们还是看看文档是怎样说的，如下

[memcpy文档](http://www.cplusplus.com/reference/cstring/memcpy/?kw=memcpy)

> ```c
> void * memcpy ( void * destination, const void * source, size_t num );
> ```
>
> Copy block of memory
>
> **拷贝内存块（拷贝内存数据）**
>
> Copies the values of *num* bytes from the location pointed to by *source* directly to the memory block pointed to by *destination*.
>
> **从source（源内存块位置）直接指向的地方开始复制num个字节的数据到destination指向的内存块位置。**
>
> The underlying type of the objects pointed to by both the *source* and *destination* pointers are irrelevant for this function; The result is a binary copy of the data.
>
> **这句话没看懂，不影响我们学这个。**
>
> The function does not check for any terminating null character in *source* - it always copies exactly *num* bytes.
>
> **这个函数不会检查'\0'，不会遇到'\0'就停下来，它就只认识要复制的num个字节数据。**
>
> To avoid overflows, the size of the arrays pointed to by both the *destination* and *source* parameters, shall be at least *num* bytes, and should not overlap (for overlapping memory blocks, [memmove](http://www.cplusplus.com/memmove) is a safer approach).
>
> **为了避免溢出，这两个数组的大小至少为num个字节，而且这两个数组内存位置不应该重叠。**

我们从文档中可以看出

1. 参数中除了要复制的`字节数num`，其他的参数类型基本都是`void*`，返回值也是`void*`
2. 该函数是从`source`的位置开始向后复制`num个字节`的数据到`destination`的内存位置。
3. 该函数在遇到` '\0'` 的时候并不会停下来。
4. `source`和`destination`不能有有任何的重叠。

#### 实现

`void*` 不能直接解引用，那么**如何复制呢？**答案是进行类型强转，转换成`char*`，一个一个字节地复制过去。

一开始得先搞一个指针变量`*rest`来存储`dest`，不然目的地址后面发生改变就不知道地址是哪里了。接下来就是主要操作了，把`src`指向的数据复制给`dest`，即`*(char*)dest = *(char*)src`，然后这两个指针都进行偏移，往后走，继续复制，就是循环下去了，直到`num`个字节复制完就结束，返回目的地址`rest`。代码如下

> 断言指针不为空是个好习惯~

```c
void* my_memcpy(void* dest, const void* src, size_t num) 
{
	assert(dest != NULL);
	assert(src != NULL);
	void* rest = dest;
	// void* 不能直接解引用，那么如何复制呢？
	// 给了num个字节，也就是需要复制num个字节
	// 那就转换成char*，一个一个字节的复制过去
	while (num--) 
	{
		*(char*)dest = *(char*)src;
		//++(char*)dest;
		//++(char*)src;
		((char*)dest)++;
		((char*)src)++;
	}
	return rest;
}
```

以上，就是memcpy函数的实现。

#### 测试代码

我们就使用下面的测试代码，测试下自己实现的my_memcpy函数，通过Debug来观察内存数据的变化。

```c
struct S
{
	char name[20];
	int age;
};

int main() 
{
	int arr1[] = { 1,2,3,4,5 };
	int arr2[5] = { 0 };

	my_memcpy(arr2, arr1, sizeof(arr1));

	struct S arr3[] = { {"LeBron", 36}, {"Kobe", 41} };
	struct S arr4[3] = { 0 };

	my_memcpy(arr4, arr3, sizeof(arr3));

	return 0;
}
```