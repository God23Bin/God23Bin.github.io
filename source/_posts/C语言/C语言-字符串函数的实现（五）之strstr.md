---
title: C语言-字符串函数的实现（五）之strstr
author: god23bin
tags:
  - C语言
  - 函数实现
categories: C
description: 关于strstr函数的实现，就是...
abbrlink: d54ed81d
date: 2021-04-17 15:59:56
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

## 字符串查找

### strstr

还是一样，先看看如何使用它，对吧哈哈哈。

```c
int main() 
{
	char* p1 = "abcdef";
	char* p2 = "def";
	// 在abcdef中找找def，找到的话返回它的地址，找不到返回空指针
	char* rest = strstr(p1, p2);
	if (rest == NULL) 
	{
		printf("子串不存在\n");
	}
	else 
	{
		printf("%s\n", rest);
	}

	return 0;
}
```

老规矩，我们还是看看文档是怎样说的，如下

[strstr文档](http://www.cplusplus.com/reference/cstring/strstr/?kw=strstr)

> ```c
> char * strstr ( const char * str1, const char * str2 );
> ```
>
> Returns a pointer to the first occurrence of *str2* in *str1*, or a null pointer if *str2* is not part of *str1*.
>
> **返回一个指针，它指向str2，该str2是在str1中第一次出现的str2，或者一个空指针，如果str2不是str1的子串。**
>
> The matching process does not include the terminating null-characters, but it stops there.
>
> **这个匹配过程不包含'\0'，但是它会停在那里。**



#### 实现

我们需要想想，它是如何实现字符串查找的？

有两个字符串，str1和str2，在str1中查找str2，那么我们需要两个指针`p1`和`p2`来进行，`p1`指向str1开头，`p2`指向str2开头，然后获取字符一一比较。

如果`*p2`为`'\0'`那么说明str2是空字符串，不能进行比较，就返回`p1`，即返回str1的地址。

如果`*p2`不为`'\0'`，就说明不是空字符串，同时，也要判断`*p1`是否为空字符串，不是就可以进行查找。

查找的话，此时，我就通过`*p1 == *p2`判断`*p1`等于`*p2`？等于就都进行偏移，即`p1++`和`p2++`，然后继续判断，这里就成了一个循环，一直循环，直到它们两个不相等跳出循环。跳出循环后，如果`*p2` 等于`'\0'`，说明已经查找到了，直接返回p2就好。如果`*p2` 不等于`'\0'`，那么就p1就进行偏移，往后移动，继续判断，这里也形成了一个循环。到这里，基本的逻辑就这样了。

下面看看代码的实现。

> 断言指针不为空是个好习惯~

```c
char* my_strstr(const char* p1, const char* p2) 
{
	// 保证指针的有效性，所以assert
	assert(p1 != NULL);
	assert(p2 != NULL);
	// 如果p2是空字符串，那就比不了
	if (*p2 == '\0') 
	{
		printf("空字符串比不了，返回p1");
		return p1;
	}
	// 真正的查找实现
	while (*p1) // 判断*p1是'\0'吗？不是就可以查找
	{
		//while (*p1 == *p2)	// 判断*p1等于*p2？等于就都进行偏移
		while ((*p1 != '\0') && (*p2 != '\0') && (*p1 == *p2) )	// 继续完善，*p1,*p2都不能是\0，遇到\0就结束了，没东西可比了
		{
			p1++;
			p2++;
		}
		if (*p2 == '\0') 
		{
			// 说明匹配到了
			return p2;
		}
		p1++;	// 不等于，那么p1往后偏移
	}
}
```

是的，到这里还没有结束，上面看似可以进行匹配了，但是代码还是有问题，比如遇到这种情况的时候

```c
int mian()
{
    char* p1 = "abbbcdef";
    char* p2 = "bbc";
    char* rest = my_strstr(p1, p2);
    return 0;
}
```

第一个字符串的第一个字符a与第二个字符串的第一个字符b进行比较，发现不相等，那么p1就进行偏移，p1往后移动

此时b与b相比，相等，那么p1和p2都进行偏移，都往后移动

还是b与b相比，相等，继续偏移

此时b与c相比，不相等，那么p1进行偏移，此时p1指向的就是第五个字符c了，后面继续比较下去，肯定都不相等，也就是说找不到bbc，但是第一个字符串里明明有bbc，就是找不到，这就是会出现的问题。

**那如何解决这个问题？**

我们知道，如果可以让p1重新回去第二个字符的位置开始比较，那么肯定就能够找到bbc，但是上面的代码中，使p1发生改变了，p1不知道第二个字符b的位置，直接从第五个字符c的位置开始了。

所以，要解决的话，我们就需要一个变量记录从哪个位置开始匹配的，然后我们**不要去改动p1**，同时保险起见，也**不要改动p2**，那么我们就可以搞多两个指针变量`s1`和`s2`，作为`p1`和`p2`的拷贝，对这两个变量进行操作，就OK了~然后搞多一个变量`current`，作为当前需要移动的指针。

```c
char* my_strstr(const char* p1, const char* p2)
{
	// 保证指针的有效性，所以assert
	assert(p1 != NULL);
	assert(p2 != NULL);
	// p1,p2不要往后动
	// 需要一个变量记录从哪个位置开始匹配
	//char* s1 = p1;	// 这里赋值无所谓，就给NULL好了
	char* s1 = NULL;
	char* s2 = NULL;
	char* current = (char*)p1;	// 这里强制类型转换，因为p1是const修饰，赋值给了char*这个没有保护的，所以强转下，不然会报警告
	// 如果p2是空字符串，那就比不了
	if (*p2 == '\0')
	{
		printf("空字符串比不了，返回p1");
		return (char*)p1;
	}
	// 真正的查找实现
	while (*current) // 判断*current是'\0'吗？不是就可以查找
	{
		s1 = current;
		s2 = (char*)p2;
	
		while ((*s1 != '\0') && (*s2 != '\0') && (*s1 == *s2))
		{
			s1++;
			s2++;
		}
		if (*s2 == '\0')
		{
			// 说明匹配到了
			return current;	// 返回子串地址
		}
		if (*s1 == '\0') 
		{
			// 如果子串比较长，那么肯定是找不到的
			return NULL;
		}
		current++;	// 不等于，那么current往后偏移
	}
	return NULL;	//找不到子串
}
```