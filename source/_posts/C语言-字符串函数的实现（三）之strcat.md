---
title: C语言-字符串函数的实现（三）之strcat
author: god23bin
tags:
  - C语言
  - 函数实现
categories: C
description: 关于strcat函数的实现，就是...
abbrlink: 5fb3a45
date: 2021-04-17 14:34:05
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

## 长度不受限制的字符串函数

### strcat

老规矩，我们还是看看文档是怎样说的，如下

[strcat文档](http://www.cplusplus.com/reference/cstring/strcat/?kw=strcat)

> ```c
> char * strcat ( char * destination, const char * source );
> ```
>
> Concatenate strings
>
> **连接字符串**
>
> Appends a copy of the *source* string to the *destination* string. The terminating null character in *destination* is overwritten by the first character of *source*, and a null-character is included at the end of the new string formed by the concatenation of both in *destination*.
>
> 追加一个源字符串的拷贝到目的字符串中。在目的字符串中的'\0'会被源字符串的第一个字符重写，然后'\0'会在新字符串的最后面。
>
> *destination* and *source* shall not overlap.
>
> 目的字符串和源字符串不应该重叠

可以知道

1. 源字符串必须以 `'\0'` 结束。
2. 虽然文档没说目标空间必须足够大，但是想一想还是可以知道的，即目标空间必须有足够大，能容纳下源字符串的内容，不然就追加不上了

#### 实现

> 断言指针不为空是个好习惯~

```c
char* my_strcat(char* dest, const char* src) 
{
	assert(dest != NULL);
	assert(src);
	char* rest = dest;
	// 1. 找到目的字符串的'\0'
	while (*dest != '\0') 
	{
		dest++;
	}
	// 2. 追加，就是字符串拷贝了，和之前的strcpy的实现一样
	while (*dest++ = *src++) 
	{
		;
	}
	return rest;
}

int main() 
{
	//char arr1[] = "hello";
	//char arr2[] = "world";

	//// 把arr2追加到arr1上
	//strcat(arr1, arr2);
	//printf("%s\n", arr1);
	// 会报错，空间不够，上面的写法是错误的
	// 可以给arr1的大小固定一个大的空间，比如arr1[30]
	char arr1[30] = "hello\0xxxxxxxxx";
	char arr2[] = "world";

	// 把arr2追加到arr1上
	//strcat(arr1, arr2);
	my_strcat(arr1, arr2);
	printf("%s\n", arr1);
	return 0;
}
```

总的来说，实现的思路就是，把源字符串追加到目的字符串的后面，从而实现字符串连接。问题就在于如何找到目的字符串的尾部，很简单，就直接找`'\0'`，找到`'\0'`就进行追加，追加就是直接复制源字符到目的空间，以此循环，复制直到遇到`'\0'`就结束，这样就完成了字符串的连接了。