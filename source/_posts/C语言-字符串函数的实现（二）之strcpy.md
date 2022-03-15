---
title: C语言-字符串函数的实现（二）之strcpy
author: god23bin
tags:
  - C语言
  - 函数实现
categories: C
description: 关于strcpy函数的实现，就是...
abbrlink: d4d21256
date: 2021-04-16 23:28:23
aplayer:
---



C语言中的字符串函数有如下这些

- 获取字符串长度
  - strlen
- 长度不受限制的字符串函数
  - strcpy
  - strcat
  - strcmp
- 长度受限制的字符串函数
  - strncpy
  - strncat
  - strncmp
- 字符串查找
  - strstr
  - strtok
- 错误信息报告
  - strerror  

接下来看看如何实现它们

## 长度不受限制的字符串函数

### strcpy

我们看看文档是怎样说的，如下

[strcpy文档](http://www.cplusplus.com/reference/cstring/strcpy/?kw=strcpy)

> ```c
> char * strcpy ( char * destination, const char * source );
> ```
>
> Copy string
>
> 字符串拷贝（字符串复制）
>
> Copies the C string pointed by *source* into the array pointed by *destination*, including the terminating null character (and stopping at that point).
>
> 复制由**字符指针source**指向的**C字符串**到另一个字符数组中，该**字符数组**由**字符指针destination指向**
>
> To avoid overflows, the size of the array pointed by *destination* shall be long enough to contain the same C string as *source* (including the terminating null character), and should not overlap in memory with *source*.
>
> 为避免溢出，由destination指向的字符数组的大小需要足够长，足够包含住源字符串（包含'\0'）

综上，可以知道

1. 会将源字符串中的 '\0' 拷贝到目标空间，源字符串必须以 '\0' 结束。
2. 目标空间必须足够大，以确保能存放源字符串。

#### 怎么实现拷贝？

```c
int main() 
{
	char arr1[] = "abcdefghi";
	char arr2[] = "bit";
	// 把arr2的内容拷贝到arr1中
	//strcpy(arr1, arr2);
	// 怎么拷贝？

	my_strcpy(arr1, arr2);
	printf("%s\n", arr1);
	return 0;
}
```

#### 实现

> 断言指针不为空是个好习惯~

```c
//char* my_strcpy(char* dest, char* src) 
// src加上const，为什么？因为我们只需要拷贝，不需要改动源字符串，防止发生修改，所以加上const修饰
char* my_strcpy(char* dest, const char* src) 
{
	assert(dest != NULL);
	assert(src != NULL);

	while (*src != '\0') 
	{
		*dest = *src;
		dest++;
		src++;
	}
	*dest = *src;	// '\0'
    // 返回目的空间的起始地址
    return dest;
}
```

源字符串拷贝到目的空间，寻找'\0'，不是'\0'的就执行`*dest = *src`，把源字符赋值给目的空间，然后两个指针都往后偏移，也就是都进行++，当`*src为'\0'`时，说明源字符串已经到结尾了，就退出这个循环，直接将`'\0'赋值给*dest`，最后返回`dest`

可以进行优化，如下

```c
char* my_strcpy(char* dest, const char* src) 
{
	assert(dest != NULL);
	assert(src != NULL);
	// 优化
	while (*src != '\0')
	{
		*dest++ = *src++;
	}
	*dest = *src;	// '\0'
    // 返回目的空间的起始地址
    return dest;
}
```

当然还可以继续优化，变得更加简洁，直接将`*dest++ = *src++`作为判断条件，同时还会执行操作，如下

```c
char* my_strcpy(char* dest, const char* src) 
{
	assert(dest != NULL);
	assert(src != NULL);
	// 优化
	// 拷贝src指向的字符串到dest指向的空间，包含'\0'
	char* rest = dest;
	while (*dest++ = *src++)
	{
		;
	}
	// 返回目的空间的起始地址
	return rest;
}
```