---
title: C语言-字符串函数的实现（一）之strlen
author: god23bin
aplayer: true
abbrlink: 57470
date: 2021-04-14 13:06:33
tags: 
- C语言
- 函数实现
categories: C
description: 关于strlen函数的实现，就是...
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

## 获取字符串长度

### strlen

我们看看文档是怎样说的，如下

[strlen文档](http://www.cplusplus.com/reference/cstring/strlen/?kw=strlen)

> ```c
> size_t strlen ( const char * str );
> ```
>
> Get string length
>
> 获取字符串长度
>
> Returns the length of the C string *str*.
>
> 返回C字符串str的长度
>
> The length of a C string is determined by the terminating null-character: A *C string* is as long as the number of characters between the beginning of the string and the terminating null character (without including the terminating null character itself).
>
> C字符串长度是由'\0'来确定的，也就是说从字符串的第一个开始只要遇到'\0'就结束长度计算（不包含'\0'）
>
> This should not be confused with the size of the array that holds the string. For example:
>
> 不用困惑你创建的数组的大小，比如这样
>
> ```c
> char mystr[100]="test string";
> ```
>
> defines an array of characters with a size of 100 `char`s, but the C string with which *mystr* has been initialized has a length of only 11 characters. Therefore, while `sizeof(mystr)` evaluates to `100`, `strlen(mystr)` returns `11`.
>
> 定义一个大小为100的数组`mystr`，然后`mystr` 就已经被初始化为一个长度为11的字符串了。所以呢， `sizeof(mystr)` 会得出 `100`, 而`strlen(mystr)` 会返回 `11`.

综上，可以知道

1. 字符串已经 '\0' 作为结束标志，strlen函数返回的是在字符串中 '\0' 前面出现的字符个数（不包含 '\0' )。
2. 该函数只认'\0'，参数指向的字符串必须要以 '\0' 结束。
3. 注意函数的返回值为size_t，是无符号的



#### 实现

strlen函数的实现有好几种。

比如

1. 计数器的方法
2. 递归
3. 指针 - 指针

接下来一一实现。

#####  1. 计数器：使用一个变量来记录 - count

> 断言指针不为空是个好习惯~

```c
int my_strlen(char* str) 
{
	int count = 0;
	assert(str != NULL);
	while (*str != '\0') // while (*str)
	{
		count++;
		str++;
	}
	return count;
}
```

就一直找'\0'，当*str不是'\0'时，就count++，str++，直到遇到'\0'停止，然后返回count就是长度了。

##### 2. 递归

> 断言指针不为空是个好习惯~

```c
int my_strlen(char* str)
{
    assert(str != NULL);
    char* p = str;
    while(*p == '\0')
    {
        return 0;
    }
    return 1 + my_strlen(p + 1);
}
```

比如传入的str地址为 1000

那么 1 + my_strlen(p + 1) 中，p + 1，指针偏移后就是1001，以此类推。

1 + 1 + my_strlen(p + 1)

1 + 1 + 1 + my_strlen(p + 1)

1 + 1 + 1 + 1 + my_strlen(p + 1)

...

1 + 1 + 1 + 1 + ... + 0

最终就可以得出长度。

##### 3. 指针-指针

> 断言指针不为空是个好习惯~

```c
int my_strlen(char* str) 
{
    assert(str != NULL);
	char* p = str;
	while (*p != '\0') 
	{
		p++;
	}
	return p - str;
}
```

把指针str的地址赋值给一个新的指针p，str作为指向起始地址的指针，不改变它，记录起始地址。

然后通过指针p进行查找'\0'，判断当前字符是否为'\0'，不是就进行p++，然后继续判断下一个字符，如此循环，直到指针p找到'\0'，然后用   **当前的指针p**  减去  **起始指针str** 进行返回，就是长度了。