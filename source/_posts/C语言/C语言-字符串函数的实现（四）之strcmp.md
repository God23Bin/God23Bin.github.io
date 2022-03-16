---
title: C语言-字符串函数的实现（四）之strcmp
author: god23bin
tags:
  - C语言
  - 函数实现
categories: C
description: 关于strcmp函数的实现，就是...
abbrlink: 7286242a
date: 2021-04-17 15:12:34
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

### strcmp

先看看如何使用它，对吧

```c
int main() 
{
	// 比较一下
	char* p1 = "abcdef";
	char* p2 = "sqwer";
	// 这里常量字符串，第一个字符的地址赋值给指针

	int ret = strcmp(p1, p2);
	if (strcmp(p1, p2)>0)
	{
		printf("p1>p2\n");
	}
	else if (strcmp(p1, p2) == 0)
	{
		printf("pa == p2\n");
	}
	else if (strcmp(p1, p2)<0)
	{
		printf("p1<p2\n")	printf("%d\n", ret);
    }
	return 0;
}
```

**它是怎么比较的？**比较长度吗？

> 不，它比的不是长度，比的是对应字符，a<s，如果第一个都是a，那么比后面的，一对一对往后比

老规矩，我们还是看看文档是怎样说的，如下

[strcmp文档](http://www.cplusplus.com/reference/cstring/strcmp/?kw=strcmp)

> ```c
> int strcmp ( const char * str1, const char * str2 );
> ```
>
> Compare two strings
>
> **比较两个字符串**
>
> Compares the C string *str1* to the C string *str2*.
>
> **str1与str2进行比较**
>
> This function starts comparing the first character of each string. If they are equal to each other, it continues with the following pairs until the characters differ or until a terminating null-character is reached.
>
> **这个函数以这两个字符串的第一个字符开始比较，如果它们彼此都相等，那么就会继续比较下一位字符，直到有字符不同，或者遇到了'\0'就会停止比较。**
>
> This function performs a binary comparison of the characters. For a function that takes into account locale-specific rules, see [strcoll](http://www.cplusplus.com/strcoll).
>
> **这个函数是以字符的二进制来进行比较的，可以说是以字符的ASCII码来进行比较。**

标准规定：

1. 如果第一个字符串大于第二个字符串，则返回大于0的数字
2. 如果第一个字符串等于第二个字符串，则返回0
3. 如果第一个字符串小于第二个字符串，则返回小于0的数字  

#### 实现

> 断言指针不为空是个好习惯~

```c
int my_strcmp(const char* str1, const char* str2)
{
	assert(str1 && str2);
	//比较
	while (*str1 == *str2)
	{
		if (*str1 == '\0')
		{
			return 0;//相等
		}
		str1++;
		str2++;
	}

	if (*str1 > *str2)
		return 1;//大于
	else
		return -1;//小于
}
```

对于函数参数，因为我们只需要比较，不需要对其进行修改，所以这里的参数就用`const`修饰，防止修改。

然后就是进行`*str1 == *str2`，判断

- 如果值一样，那么就要进行偏移，即`str1++`和`str2++`，使指针往后移动，进行下一个字符的比较，如果一直相等，那么直到遇到`'\0'`，就说明比较的两个字符串是相等的，直接返回0；

- 如果值不一样了，那么即直接进行`*str1`和`*str2`的比较，`*str1 > *str2`就返回1，否则返回-1

实际上，可以进行优化，最后的判断可以改为做差的形式，来比较大小，就不用写if-else了`(*str1 - *str2)`，如下

```c
int my_strcmp(const char* str1, const char* str2)
{
	assert(str1 && str2);
	//比较
	while (*str1 == *str2)
	{
		if (*str1 == '\0')
		{
			return 0;//相等
		}
		str1++;
		str2++;
	}

	return (*str1 - *str2);
}
```

测试的主函数

```c
int main()
{
	char* p1 = "abcdef";
	char* p2 = "qwert";
	int ret = my_strcmp(p1, p2);
	printf("ret = %d\n", ret);

	return 0;
}
```