---
title: 시퀀스 컨테이너의 삭제와 이터레이터 무효화(erase(), remove())
date: 2023-12-01 23:10:58 +0900
categories:
  - 컴퓨터 공학
  - 자료구조
tags:
  - c
  - 시퀀스 컨테이너
  - 이터레이터
  - 자료구조
  - 메모리
math: false
type: DataStructure
keywords:
  - iterator invalidation
  - remove()
  - 이터레이터 무효화
  - erase()
---

vector를 사용하다 보면 다음과 같은 에러를 한 번씩은 봤을 수 있습니다.

`erase()`를 쓰거나 `insert()`를 쓰다 보면 가끔 **invalidated iterator**라는 표현이 나옵니다.

![Expression: can’t increment invalidated vector iterator](https://i.postimg.cc/y6fk7wQ7/image.png){: w="412" h="328"}
_Expression: can’t increment invalidated vector iterator_

만약, 본 적 없다면, 이미 알고 잘 쓰고 계시거나 논리적으로 문제가 생길 것 같은 작업을 잘 피해 오셨을 것으로 생각됩니다.

다음 코드는 잘못된 코드입니다. 어떻게 고쳐야 할까요?

```cpp
std::vector<int> vec = { 1, 2, 3, 4, 5 };

std::vector<int>::iterator vi = vec.begin();
std::vector<int>::iterator ve = vec.end();

for (; vi != ve; vi++)
{
	if (*vi == 3)
	{
		vec.erase(vi);
	}
}
```

# 이터레이터 무효화

이터레이터에는 <span class="keyword">**이터레이터 무효화(iterator invalidation)**</span>라는 게 존재합니다.

이터레이터는 특정 메모리 위치를 가리키는 역할로, 기본적으로 포인터와 유사하게 동작합니다.

하지만 사실, 이터레이터는 단순한 포인터보다 더 복잡한 동작을 수행합니다. 이터레이터는 컨테이너와 연결되어 컨테이너의 상태 변화에 영향을 받습니다.

그리고 <span class="keyword">**이터레이터 무효화**</span>는 특정 컨테이너의 요소를 가리키는 **이터레이터가 컨테이너의 구조가 변경되어 <span class="font_highlight">더 이상 유효한 메모리 위치를 가리키지 않을 것 같을 때</span> 발생**합니다.

즉, 이터레이터가 유효하지 않은 위치를 가리키게 되어 프로그래머가 실수로 사용해 **정의되지 않은 동작(undefined behavior)을 일으키는 것을 방지**하기 위한 것입니다.

## vector에서의 요소 삽입/삭제와 이터레이터 무효화

**`std::vector`**에서 요소를 삭제하면, **삭제된 요소 뒤에 있는 모든 요소들이 메모리상에서 앞으로 이동**합니다. 이 과정에서 이동되는 요소들의 실제 메모리 주소가 변경됩니다.

그래서 **`erase()`**는 **삭제된 요소와 그 이후의 모든 요소**(`end()` 포함)를 **무효화** 합니다.

같은 이유로 **`insert()`**는 **삽입 지점 이후의 모든 이터레이터**(`end()` 포함)를 **무효화** 합니다.

게다가 당연한 말이지만, 벡터의 용량이 변경되어 **메모리를 재할당해야 하는 경우 모든 이터레이터가 무효화** 됩니다.

## 다른 컨테이너에서도 발생하는 이터레이터 무효화

사실, 이것 말고도 이터레이터 무효화는 메모리가 연속된 컨테이너에서 발생합니다.

일단, 대부분의 컨테이너에서 삭제된 요소를 가리키는 이터레이터는 당연히 무효화 되고,

vector와는 다르지만, 연속된 메모리 구간을 사용하는 deque도 이터레이터 무효화가 발생할 수 있습니다.

맨 끝에서 삽입이 일어나는 경우나 중간에 삽입/삭제가 일어나는 경우가 그 경우입니다.

자세한 내용은 [Containers library - cppreference.com](https://en.cppreference.com/w/cpp/container){: target="_blank"}를 참조해  주시면 좋겠습니다.

## 해결 방안

위 코드를 다시 보겠습니다.

```cpp
std::vector<int> vec = { 1, 2, 3, 4, 5 };

std::vector<int>::iterator vi = vec.begin();
std::vector<int>::iterator ve = vec.end();

for (; vi != ve; vi++)
{
	if (*vi == 3)
	{
		vec.erase(vi);
	}
}
```

이 코드에서 문제점은 다음과 같습니다.

1. `ve = vec.end()`으로 초기화해서 사용했기 때문에, `erase()` 이후 **무효화** 되어 `vi != ve` 표현식이 더 이상 유효하지 않다.
2. `vec.erase(vi);`에서 `vi`는 삭제된 위치를 가리키고 있었기 때문에 `erase()` 이후 **무효화** 됩니다.

그렇다면, 무효화 되는 이터레이터를 다시 유효한 이터레이터로 바꿔주면 됩니다.

```cpp
for (; vi != vec.end(); vi++)
{
	if (*vi == 3)
	{
		vi = vec.erase(vi);
	}
}
```

다행히 `erase()`는 삭제할 요소의 바로 다음 이터레이터를 반환합니다.

그러므로 이렇게 수정하면 동작하게 됩니다.

1. `end()`를 초기화하지 않고 매 조건 검사 때 함수를 호출해서 비교한다.
2. 삭제 후 유효한 이터레이터를 사용하기 위해 반환된 이터레이터를 사용한다.

하지만, 이런 방식에도 여전히 문제가 존재합니다. 그게 무엇인지는 erase() 함수의 특징을 보면서 알아보겠습니다.

# erase() 함수의 특징

대부분의 시퀀스 컨테이너에서 `erase()`는 다음과 같이 동작합니다.

삭제된 요소 이후의 모든 요소들이 앞으로 이동하고 총 **size**가 줄어들게 됩니다. (vector와 같은 경우 **capacity**를 줄이지 않습니다.)

![lsit_erase.webp](https://i.postimg.cc/yYtxZ0C5/02.webp){: w="1000" h="315"}

물론, 삭제될 요소를 가리키던 Iterator는 무효화 되고, 새로운 요소를 가리키는 Iterator를 반환하지만, 지금 여기에서 포인트는 가리키던 요소가 **삭제된 요소 바로 다음 요소를 가리키는 이터레이터**라는 점입니다.

## 반복문에서의 erase()

```cpp
std::vector<int> vec = { 1, 2, 3, 3, 4, 5 };

for (; vi != vec.end(); vi++)
{
	if (*vi == 3)
	{
		vi = vec.erase(vi);
	}
}
```

![Untitled](https://i.postimg.cc/c4r6CX2w/03.png){: w="114" h="31"}

만약, 이런 형태(3, 3 이 연속된 형태)의 자료를 반복문에 써본다고 한다면 바로 알 수 있겠지만, `erase()` **이후 증감문이 실행되면서 검사 할 요소를 하나 건너뛰게 됩니다**.

이는, 반복문을 사용하면서 **<span class="font_highlight">현재 요소의 수정이 이후 요소들에 영향</span>을 미치는 형태의 자료구조에서 자주 발생하는 문제**입니다.

따라서, 이를 원하는 동작으로 쓰기 위해서는 다음과 같이 써야 할 것입니다.

```cpp
std::vector<int> vec = { 1, 2, 3, 3, 4, 5 };

for (; vi != vec.end();)
{
	if (*vi == 3)
	{
		vi = vec.erase(vi);
	}
	else
	{
		vi++;
	}
}
```

![Untitled](https://i.postimg.cc/9Q9MtzWf/05.png){: w="113" h="39"}

과연, 요소를 삭제할 때마다 이런 식으로 이터레이터 무효화와 이터레이터의 특징을 생각해 가며 프로그래밍해야 할까요?

STL의 알고리즘에는 이런 문제를 해결해 줄 수 있는 `remove()`가 존재합니다.

---

# remove() 함수의 특징

STL의 알고리즘에는 `remove()` 및 `remove_if()`라는 알고리즘이 존재합니다.

이 알고리즘은 **컨테이너의 크기를 변경하지 않고, <span class="font_highlight">제거될 요소들을 컨테이너 범위의 끝으로 이동</span>시키는 방식으로 동작**합니다. (실제 동작 방식은 구현에 따라 다를 수 있습니다.)

그 이후 제거되지 않은 요소들의 새로운 끝(**제거하고자 하는 요소들의 시작**) 부분을 가리키는 이터레이터를 반환합니다.

![Untitled](https://i.postimg.cc/GpJmVNTw/04.png){: w="1000" h="360"}

이런 식으로 동작하는 이유는 STL의 알고리즘이기 때문에 모든 컨테이너에서 동작할 수 있도록 일반적이면서도 안전한 동작을 해야 하기 때문입니다.

어쨌든, 중요한 건 `remove()`가 실제 삭제될 요소들의 시작 부분을 알려주었으니, 나머지를 실제로 제거하기만 하면 된다는 것입니다.

# remove() 함수와 erase() 함수의 조합

사실 `erase()`는 범위 기반 삭제도 오버로딩으로 구현되어 있습니다.

```cpp
erase(iterator _First, iterator _Last)
```

즉, 두 이터레이터 사이의 값들을 모두 삭제할 수 있다는 것입니다.

이걸 이용하면, 이런 식으로 특정 요소들을 한 번에 삭제할 수 있습니다.

```cpp
std::vector<int> vec = { 1, 2, 3, 3, 4, 5 };

std::vector<int>::iterator vi = vec.begin();
vec.erase(remove(vec.begin(), vec.end(), 3), vec.end());
```

![Untitled](https://i.postimg.cc/9Q9MtzWf/05.png){: w="113" h="39"}

`remove()` 알고리즘으로 삭제하고자 하는 요소들을 맨 뒤에 모아두고,

`erase(iter, end())`로 실제 삭제할 요소들을 모조리 삭제해 주면 매우 쉽게 삭제할 수 있는 것입니다.

---

# 마무리

이 글은 처음에 vector 컨테이너 포스트에 쓸까, 생각했다가 list나 deque 같은 시퀀스 컨테이너들에 해당하는 내용이 많이 들어있을 것 같아 한 번에 모아서 썼습니다.

이 포스트의 중점 내용은 remove() 알고리즘과 erase()에 대한 글이긴 하지만, 아래 내용도 중요합니다.

- 이터레이터 무효화(iterator invalidation)
- 반복문에서 현재 요소의 수정이 이후 요소들에 영향을 미치는 형태의 자료구조에서 발생하는 문제

remove()와 erase()를 이해하고 사용하는 것도 중요하지만, 이터레이터를 사용하다가 발생할 문제, 반복문을 사용하다가 발생하는 문제를 발견하고 수정하는 것도 중요하다고 생각했습니다.

이 포스트를 읽고 나중에 프로그래밍을 하다가 이런 문제가 발생했을 때, 도움이 되길 바라며, 마무리하겠습니다.

---

# 참고

이터레이터 무효화: [Containers library - cppreference.com](https://en.cppreference.com/w/cpp/container)