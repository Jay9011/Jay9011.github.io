---
title: 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)
date: 2024-01-17 16:55:58 +0900
categories:
    - 컴퓨터 공학
    - 소프트웨어 공학
tags:
    - 소프트웨어 공학
    - 포인터
    - 참조
math: false
type: SoftwareEngineering
keywords:
    - Shallow Copy
    - 깊은 복사
    - 얕은 복사
    - Deep Copy
---

# 얕은 복사 (Shallow Copy)

얕은 복사는 <span class="font_highlight">객체의 **모든 필드 값을 그대로 복사**하는 것</span>입니다.

하지만, **주소를 참조하는 필드**가 존재한다면, 복사된 객체와 원본 객체는 **같은 메모리 주소**를 참조하게 됩니다.

즉, 얕은 복사는 참조 대상을 새롭게 복사하지 않으므로, <span class="font_highlight">원본과 복사본이 동일한 대상을 참조</span>하게 됩니다.

결과적으로, 하나의 객체에서 포인터가 가리키는 값을 변경하면 다른 객체에도 영향을 미치게 됩니다.

```cpp
class Shallow 
{
public:
	int* data;

	Shallow(int d) 
	{
		data = new int; // 동적 메모리 할당
		*data = d;
	}
	~Shallow() 
	{
		delete data;
	}

	// 복사 생성자 (얕은 복사)
	Shallow(const Shallow& source) : data(source.data) {}
};

int main() 
{
	Shallow obj1(2023);
	Shallow obj2 = obj1; // 얕은 복사 발생

	// obj1의 data 변경
	*(obj1.data) = 2024;

	cout << "Original data after change: " << *obj1.data << endl;	// 2024
	cout << "Copied data after change: " << *obj2.data << endl;		// 2024
}
```
![얕은 복사](https://i.postimg.cc/jqDT8Tqh/image.webp){: w="450" h="200"}

# 깊은 복사 (Deep Copy)

깊은 복사는 객체의 **모든 필드를 복사**하되, <span class="font_highlight">**참조하고 있는 객체**들까지도 모두 **새롭게 복사**하는 것</span>입니다.

따라서, 복사된 객체는 원본 객체와 **독립적으로 존재**하며, 하나의 객체에서 참조 대상의 상태를 변경해도 다른 객체에는 영향을 미치지 않습니다.

```cpp
class Deep
{
public:
	int* data;

	Deep(int d)
	{
		data = new int; // 동적 메모리 할당
		*data = d;
	}
	~Deep()
	{
		delete data;
	}

	// 복사 생성자 (깊은 복사)
	Deep(const Deep& source)
	{
		data = new int; // 새로운 메모리 할당
		*data = *source.data;
	}
};
int main()
{
	Deep obj3(2023);
	Deep obj4 = obj3; // 깊은 복사 발생

	// obj3의 data 변경
	*(obj3.data) = 2024;
	cout << "Original data after change: " << *obj3.data << endl;	// 2024
	cout << "Copied data after change: " << *obj4.data << endl;		// 2023
}
```

![깊은 복사](https://i.postimg.cc/bY4hW5sf/image.webp){: w="450" h="197"}