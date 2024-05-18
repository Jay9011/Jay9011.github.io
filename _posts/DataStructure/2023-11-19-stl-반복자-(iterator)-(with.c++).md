---
title: STL - 반복자 (Iterator) (with.C++)
date: 2023-11-19 19:22:52 +0900
categories:
  - 컴퓨터 공학
  - 자료구조
tags:
  - 자료구조
math: false
type: DataStructure
keywords:
  - STL
  - 반복자
  - 이터레이터
  - Iterator
---

반복자, 흔히 <span class="keyword">**이터레이터(Iterator)**</span>라 불리는 이것은 STL(Standard Template Library)에서 매우 중요한 역할을 합니다. 
<br> (이후 반복자라는 단어 보다는 이터레이터라는 단어를 사용하겠습니다.)

이터레이터는 **<span class="font_highlight">컨테이너</span>에 저장된 데이터를 순회하여 <span class="font_highlight">각 데이터에 대한 접근을 제공</span>하는 객체**입니다.

단순히 array나 vector와 같은 컨테이너만 배웠다면, index로 접근하면 되지 왜 이터레이터가 필요한지 의문이 들 수 있겠지만, list나 map과 같은 컨테이너를 순차적으로 탐색하기 위해서는 각 컨테이너에 대한 구조 파악과 함께 순회 방법을 직접 작성해야 할 것입니다.

하지만, 이 이터레이터를 사용하면 컨테이너의 내부 구조에 관계없이 일관된 방식으로 요소에 접근할 수 있습니다.

---

# Iterator의 주요 특징

## 1. 일반화된 접근

우선, **<span class="font_highlight">다양한 종류의 컨테이너</span>에 대해 동일한 방식으로 탐색하거나 접근**할 수 있는 방안이 필요할 것입니다.

예를 들어, 모든 이터레이터는 `++`로 다음 요소로 이동, `*`로 요소에 접근하는 방법을 제공합니다.

## 2. 알고리즘과의 호환성

STL의 주요 구성 요소로는 데이터를 저장하는 컨테이너, 데이터에 접근하는 이터레이터, 데이터를 처리하는 알고리즘이 있습니다.

이때, **이터레이터는 <span class="font_highlight">컨테이너와 알고리즘 사이를 연결</span>해주는 중요한 역할**을 하므로, **다양한 STL 알고리즘에 대해서도 일관된 방식으로 데이터에 접근하고 조작할 수 있는 방법**이 필요 할 것입니다.

예를 들어, 정렬, 검색 또는 변환과 같은 알고리즘들은 이터레이터를 통해 컨테이너의 데이터에 접근합니다. 이 과정에서 이터레이터는 필요한 연산(예: `비교 연산`, `대입 연산`)을 수행할 수 있도록 해당 연산자를 지원해야 합니다.

## 3. 다양한 종류

이터레이터는 **<span class="font_highlight">다양한 방식의 탐색</span>을 위해 다양한 종류의 이터레이터**를 제공합니다.

예를 들어, 순서 대로 탐색하기 위한 순방향 이터레이터나, 그 반대로 역순으로 탐색하기 위한 역방향 이터레이터도 제공됩니다.

---

# Iterator의 종류

이론상 이터레이터는 다음과 같은 종류들이 있습니다.

1. <span class="important">**입력**</span> 이터레이터(input iterator)
    1. 가장 단순한 형태의 이터레이터로 **읽기 전용** 이터레이터입니다.
    2. **한 방향으로만 이동**할 수 있으며, 각 요소를 한 번씩만 읽습니다.
2. <span class="important">**출력**</span> 이터레이터(output iterator)
    1. 입력 이터레이터와는 반대로, 컨테이너의 값을 변경하기만 하는 용도로, **쓰기 전용** 이터레이터입니다.
    2. 이 역시 **한 방향으로만 이동**할 수 있으며, 각 요소를 한 번씩만 읽습니다.
3. <span class="important">**순방향**</span> 이터레이터(forward iterator)
    1. **읽기와 쓰기**가 가능한 이터레이터입니다.
    2. **한 방향**으로만 이동 할 수 있습니다.
4. <span class="important">**양방향**</span> 이터레이터(bidirectional iterator)
    1. **읽기와 쓰기** 모두 가능하며, 
    2. **양 방향**으로 이동할 수 있습니다. (`++`, `- -`)
5. <span class="important">**임의 접근**</span> 이터레이터(random access iterator)
    1. 최상위 레벨의 이터레이터로 가장 많은 기능을 제공하는 이터레이터입니다.
    2. 양방향 이터레이터의 모든 기능을 포함하며(**읽기와 쓰기**가 가능하며, **양 방향**으로 이동),
    3. 배열과 유사하게 **임의 위치**의 요소에 접근할 수 있습니다.
6. <span class="important">**역방향**</span> 이터레이터(reverse iterator)
    1. **읽기와 쓰기**가 가능한 이터레이터입니다.
    2. 순회시 **역순**으로 순회합니다.
7. <span class="important">**스트림**</span> 이터레이터(stream iterator)
    1. **입력 또는 출력 스트림**과 연결되어, 스트림에 데이터를 읽거나 쓰는 데 사용됩니다.
8. <span class="important">**삽입**</span> 이터레이터(insert iterator)
    1. 일반적인 이터레이터들은 요소를 직접 가리키기 때문에, 삽입이 아니라 덮어쓰기가 되지만,
    2. 이 이터레이터는 특정 위치에 **요소를 삽입**하는 데 사용됩니다.
9. <span class="important">**상수**</span> 이터레이터(constant iterator)
    1. 이터레이터가 가리키는 값을 상수화 시켜 **읽기 전용**으로 접근합니다.

다음은 C++에서 사용되는 Iterator를 나름대로 정리 해봤습니다.

## 1. 순방향 Iterator

순방향 이터레이터는 **읽기와 쓰기**가 가능하며 **한 방향**으로만 이동 할 수 있습니다.

C++의 순방향 이터레이터는 forward_list를 통해 확인할 수 있습니다.

```cpp
#include <iterator>
#include <forward_list>
#include <iostream>

using namespace std;

int main()
{
	forward_list<int> flst = { 1, 2, 3 };
	forward_list<int>::iterator it2 = flst.begin();
	it2++;
	// it2--; // 단방향 이터레이터는 -- 연산자를 지원하지 않음

	*it2 = 4;
	cout << "*it2 = " << *it2 << endl; // *it2 = 4
}
```

## 2. 양방향 iterator와 역 양방향 Iterator

양방향 이터레이터는 **읽기와 쓰기** 모두 가능하며, **양 방향**으로 이동할 수 있습니다. (`++`, `--`)

또한, 역방향으로 탐색 가능한 컨테이너는 역방향 iterator도 지원합니다.

C++의 양방향 이터레이터는 list를 통해 확인할 수 있습니다.

```cpp
#include <iterator>
#include <list>
#include <iostream>

using namespace std;

int main()
{
	list<int> lst = { 1, 2, 3 };
	list<int>::iterator it1 = lst.begin();
	list<int>::reverse_iterator rit1 = lst.rbegin();
	it1++; it1--;
	rit1++; rit1--;
	//cout << "it1[2] = " << it1[2] << endl; // 임의 접근 불가능

	cout << "*it1 = " << *it1 << endl; // *it1 = 1
}
```

## 3. 임의 접근 Iterator와 역방향 임의 접근 Iterator

임의 접근 이터레이터는 **읽기와 쓰기**가 가능하며, **양 방향**으로 이동 할 수 있습니다.

또한, 임의 접근 가능한 컨테이너에서 배열과 유사하게 **임의 위치**의 요소에 접근할 수 있습니다.

C++의 임의 접근 이터레이터는 vector를 통해 확인할 수 있습니다.

```cpp
#include <iterator>
#include <vector>
#include <iostream>

using namespace std;

int main()
{
	vector<int> vec = { 1, 2, 3 };
	vector<int>::iterator it = vec.begin();
	vector<int>::reverse_iterator rit = vec.rbegin();

	it++; it--;
	cout << "it[2] = " << it[2] << endl;
	rit++; rit--;
	cout << "rit[2] = " << rit[2] << endl;
}
```

![임의 접근 이터레이터와 역방향 임의접근 이터레이터](https://i.postimg.cc/Kzk89zbj/image.png){: w="140" h="42"}

## 4. 입•출력 스트림 Iterator

입력 스트림 이터레이터(istream_iterator)와 출력 스트림 이터레이터(ostream_iterator)는 스트림과 상호작용하는 데 사용되는 이터레이터입니다.

istream_iterator는 입력 스트림(예: `std::cin`)에서 데이터를 읽는 데 사용됩니다.

이 이터레이터를 사용하면 **스트림에서 데이터를 읽어 컨테이너에 저장하거나, 알고리즘에 직접 전달**할 수 있습니다.

반대로, ostream_iterator는 출력 스트림(예: `std::cout`)에 데이터를 쓰는 데 사용됩니다.

이 이터레이터를 사용하면 **컨테이너의 내용을 스트림에 쉽게 출력**할 수 있습니다.

```cpp
#include <iterator>
#include <iostream>

using namespace std;

int main()
{
	istream_iterator<int> in_it(cin);
	cout << "in_it = " << *in_it << endl;

	ostream_iterator<int> out_it(cout, " (out_it)\n");
	*out_it = 1;
	*out_it = 2;
}
```

![입출력 스트림 이터레이터](https://i.postimg.cc/BvwvcFdR/image.png){: w="219" h="86"}

위 코드는 표준 입력(`cin`)에서 정수를 하나 읽어 `in_it`를 통해 접근하고 `*in_it`를 통해 출력합니다.

그리고, `cout`을 사용하여 출력 스트림에 정수 값을 씁니다. 첫 번째 줄은 1을 출력하고, 두 번째 줄은 2를 출력합니다. 각 출력 후에는 지정한 구분자인 `" (out_it)\n"` 문자열이 추가됩니다.

## 5. 삽입 Iterator

삽입 이터레이터는 특정 위치에 **요소를 삽입**하는 데 사용됩니다.

대부분의 컨테이너에서 지원되지만, **`std::array`**(크기가 고정된 것)와 같이 삽입이 제한된 컨테이너에서의 사용은 조심해야 합니다.

다음은 C++에서 vector를 사용한 삽입 Iterator의 예제입니다.

```cpp
#include <iterator>
#include <vector>
#include <iostream>

using namespace std;

int main()
{
	vector<int> vec = { 1, 2, 3 };
	vector<int>::iterator it = vec.begin();
	insert_iterator<vector<int>> iit(vec, vec.begin() + 1);
	*iit = 4;
	for (it = vec.begin(); it != vec.end(); it++)
	{
		cout << *it << " ";
	} // 1 4 2 3
}
```

![삽입 이터레이터](https://i.postimg.cc/Bvv6Kpwx/image.png){: w="85" h="22"}

## 6. 상수 Iterator

상수 이터레이터는 이터레이터가 가리키는 값을 상수화 시켜 **읽기 전용**으로 접근합니다.

C++에서는 대부분의 이터레이터가 지원되는 컨테이너에 같이 정의되어 있습니다.

```cpp
#include <iterator>
#include <vector>
#include <iostream>

using namespace std;

int main()
{
	vector<int> vec = { 1, 2, 3 };
	vector<int>::const_iterator cit = vec.cbegin();
	vector<int>::const_reverse_iterator crit = vec.crbegin();

	cit++; cit--;
	// *cit = 4; // cit는 상수 이터레이터라서 쓰기가 불가능
	cout << "cit[2] = " << cit[2] << endl;
	crit++; crit--;
	// *crit = 4; // crit는 상수 이터레이터라서 쓰기가 불가능
	cout << "crit[2] = " << crit[2] << endl;
}
```

---

# 참고

[STL - Standard Template Library 란?](/posts/stl-standard-template-library-%EB%9E%80/)

[STL - 컨테이너 (Container)](/posts/stl-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-(container)/)

[STL - 알고리즘 (Algorithm)](/posts/stl-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-(algorithm)/)