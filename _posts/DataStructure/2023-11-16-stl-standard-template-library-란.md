---
title: STL - Standard Template Library 란?
date: 2023-11-16 20:58:39 +0900
categories:
  - 컴퓨터 과학
  - 자료구조
tags:
  - 알고리즘
  - 유연성
  - 이터레이터
  - 자료구조
  - 컨테이너
  - c
math: false
type: DataStructure
keywords:
  - Generic Programming
  - STL
  - 결합성
  - 모듈성
  - 반복자
  - 알고리즘
  - 일반화 프로그래밍
  - 컨테이너
  - Iterator
---

<span class="keyword">**STL(Standard Template Library)**</span>은 <span class="font_highlight">**일반화 프로그래밍(Generic Programming)**</span> 기법을 활용하여, **<span class="font_highlight">데이터 타입에 얽매이지 않고 유연하게 적용</span>될 수 있는 자료 구조와 알고리즘을 제공하는 라이브러리**입니다.

여기서 <span class="keyword">**일반화 프로그래밍(Generic Programming)**</span>이란, **<span class="font_highlight">데이터 타입에서 독립적인 코드</span>를 작성할 수 있게 해주는 프로그래밍 패러다임**으로, C++의 템플릿(Template)을 통해 구현됩니다.

STL(표준 템플릿 라이브러리)은 이러한 프로그래밍 패러다임을 통해, **데이터 구조와 알고리즘을 유연하게 재사용할 수 있는 강력한 도구**입니다.

---

# STL의 주요 구성 요소

STL의 중요한 특징 중 하나로 **모듈성**과 **결합성**이 있습니다.

STL은 자료를 저장하는 자료 구조, 자료를 탐색하는 탐색기, 자료를 통해 어떤 문제를 해결하는 알고리즘 등이 서로 독립적으로, 또한 함께 사용될 수 있도록 설계되었습니다.

다시 말해, 이러한 구성 요소들은 모듈처럼 나눠져 있으며, 다양한 방식으로 구성 요소들을 조합해 사용할 수 있습니다.

1. <span class="keyword">**컨테이너 (Container)**</span>: **자료를 저장**하는 자료 구조.
2. <span class="keyword">**반복자 (Iterator)**</span>: 자료 구조를 **탐색**할 수 있는 탐색 도구.
3. <span class="keyword">**알고리즘 (Algorithm)**</span>: 데이터를 **처리**하고 **조작**하는 방법.

## 컨테이너 (Container)

컨테이너는 **같은 타입의 여러 <span class="font_highlight">객체를 저장</span>하는 자료 구조**로, 컨테이너를 초기화 할 때 데이터 타입을 명시함으로써, 다양한 종류의 데이터 구조를 만들 수 있습니다.

주요 컨테이너의 유형은 다음과 같습니다.

1. **시퀀스 컨테이너(Sequence container)** (ex. vector, list, deque)
2. **연관 컨테이너(Associative container)** (ex. set, map)
3. **어댑터 컨테이너(Adapter container)** (ex. stack, queue, priority_queue)

## 알고리즘 (Algorithm)

STL 알고리즘은 **<span class="font_highlight">데이터를 처리하고 조작</span>하는 데 사용되는 함수 템플릿의 집합**입니다.

이 알고리즘들은 주로 컨테이너와 함께 작동하고, 다음과 같은 알고리즘들이 존재합니다.

1. **정렬(Sort)**
2. **검색(Search)**
3. **변환(Transform)**
4. 그 외에 **합(Accumulation)**이나 **집합 연산(Set operations)** 같은 알고리즘 등…

## 반복자 (Iterator)

반복자. 흔히 이터레이터라고 부르는 이것은 **포인터와 유사한 개념**으로, **컨테이너의 요소를 순회**할 수 있게 해줍니다.

이터레이터는 **컨테이너에 저장된 <span class="font_highlight">데이터를 순회</span>하여 각 <span class="font_highlight">데이터에 대한 접근을 제공</span>하는 객체**로, 컨테이터의 시작과 끝이라는 개념이 존재합니다. (begin, end)

주요 이터레이터 유형은 다음과 같습니다.

1. 입력
2. 출력
3. 순방향
4. 역방향
5. 임의 접근


---

# 참고

[STL - 컨테이너 (Container)](/posts/stl-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-(container)/)

[STL - 반복자 (Iterator) (with.C++)](/posts/stl-%EB%B0%98%EB%B3%B5%EC%9E%90-(iterator)-(with.c++)/)

[STL - 알고리즘 (Algorithm)](/posts/stl-%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98-(algorithm)/)