---
title: C语言-内存函数的实现（二）之memmove
author: god23bin
tags:
  - C语言
  - 函数实现
categories: C
description: 关于memmove函数的实现，就是...
abbrlink: 290f417a
date: 2021-04-20 17:36:23
aplayer:
---

C语言中的内存函数有如下这些

- memcpy
- memmove
- memcmp
- memset

## 下面看看memmove函数

### memmove

**为什么会需要memmove函数？**

```c
int main() 
{
	int arr[] = { 1,2,3,4,5,6,7,8,9,10 };
	int i = 0;
	// 想把12345 拷贝到 34567上去
	// 应该打印 1 2 1 2 3 4 5 8 9 10
	my_memcpy(arr + 2, arr, 20);

	for (i = 0; i < 10; i++) 
	{
		printf("%d ", arr[i]);
	}
	// 但是输出 1 2 1 2 1 2 1 8 9 10

	return 0;
}
```

上面会输出 1 2 1 2 1 2 1 8 9 10，我们来看看为什么会出现这样的结果。

我这里画了张图，方便理解。

![出现1 2 1 2 1 1 2 8 9 10](https://raw.githubusercontent.com/god23bin/pic-bed/master/img/20220516144336.png)

因为拷贝的地方重叠了，使原来的数据（3 4 5）被覆盖了，导致最后出来的结果不是我们想要的。

也就是说，如果拷贝的地方重叠了，那么就会出现这种情况。

**那么如何解决呢？**答案就是从后往前拷贝，指针从最后的地方开始操作。

还是上一张图

![从后往前拷贝解决1 2 1 2 1 1 2 8 9 10](https://raw.githubusercontent.com/god23bin/pic-bed/master/img/20220516144343.png)

这样，就得出了我们想要的结果。

但是呢，也不能一概而论，就全部都是从后往前拷贝，还是得分情况的，具体就是看`destination`和`source`的位置关系。

![destination和source位置关系](https://raw.githubusercontent.com/god23bin/pic-bed/master/img/20220516144349.png)

回到最开始的问题，**为什么会需要memmove函数？**，因为memmove这个函数可以处理这种重叠拷贝。

老规矩，我们还是看看文档是怎样说的，如下

[memmove文档](http://www.cplusplus.com/reference/cstring/memmove/?kw=memmove)

> ```c
> void * memmove ( void * destination, const void * source, size_t num );
> ```
>
> Move block of memory
>
> **移动内存块（移动内存数据）**
>
> Copies the values of *num* bytes from the location pointed by *source* to the memory block pointed by *destination*. Copying takes place as if an intermediate buffer were used, allowing the *destination* and *source* to overlap.
>
> **从source（源内存块位置）直接指向的地方开始复制num个字节的数据到destination指向的内存块位置。然后复制就像使用了中间缓冲区一样，允许destination和source重叠。**
>
> The underlying type of the objects pointed by both the *source* and *destination* pointers are irrelevant for this function; The result is a binary copy of the data.
>
> **这句话没看懂，不影响我们学这个。**
>
> The function does not check for any terminating null character in *source* - it always copies exactly *num* bytes.
>
> **这个函数不会检查'\0'，不会遇到'\0'就停下来，它就只认识要复制的num个字节数据。**
>
> To avoid overflows, the size of the arrays pointed by both the *destination* and *source* parameters, shall be at least *num* bytes.
>
> **为了避免溢出，这两个数组的大小至少为num个字节。**

可以看出，memmove和memcpy的唯一区别就是，memmove函数处理的源内存块和目标内存块是可以重叠的。
也就是说，如果源空间和目标空间出现重叠，就得使用memmove函数处理。

#### 实现

> 断言指针不为空是个好习惯~

```c
void* my_memmove(void* dest, void* src, size_t num) 
{
	//dest落在了src的左边，从前往后拷贝
	//dest落在了src的右边，同时没有超过那个重叠的边界的时候，从后往前拷贝
	assert(dest != NULL);
	assert(src != NULL);
	void* rest = dest;
	// void* 不能直接解引用，那么如何复制呢？
	// 给了num个字节，也就是需要复制num个字节
	// 那就转换成char*，一个一个字节的复制过去
	if (dest < src) 
	//if (dest < src || dest > (char*)src + num) 
	{
		//dest落在了src的左边，从前往后拷
		while (num--)
		{
			*(char*)dest = *(char*)src;
			//++(char*)dest;
			//++(char*)src;
			((char*)dest)++;
			((char*)src)++;
		}
	}
	else 
	{
		// 从后往前拷
		// 找到最后一个字节
		while (num--) 
		{
			*((char*)dest + num) = *((char*)src + num);
		}

	}
	return rest;
}
```