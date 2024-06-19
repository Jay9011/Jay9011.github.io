---
title: 딕셔너리(Dictionary)
date: 2024-06-14 22:50:55 +0900
categories:
  - 컴퓨터 공학
  - 자료구조
tags:
  - 자료구조
  - 컨테이너
math: true
type: DataStructure
---

- 라이브러리
    - C#: `System.Collections.Generic`, `Dictionary<K, V>`
    - Python: `{:}`, `dict()`
    - C++: `<unordered_map>`, `std::unordered_map<K, V>`
	    - C++에서는 딕셔너리와 유사한 자료구조로 해시 테이블을 사용하는 `unordered_map`이 있습니다.
	    - `std::map`도 존재하는데, 이는 내부적으로 이진 검색 트리를 사용하여 데이터를 저장합니다.
	- JAVA: `java.util`, `HashMap<K, V>`
		- JAVA에서는 딕셔너리와 유사한 자료구조로 `HashMap`이 있습니다.
- 탐색 시간
    - 평균: $O(1)$
    - 최악: $O(N)$ (충돌 발생시)
- 요소 추가/삭제
    - 평균: $O(1)$
    - 최악: $O(N)$ (충돌 발생시)

딕셔너리(Dictionary) 자료구조는 <span class="important">**키-값 쌍(key-value pairs)**</span>으로 데이터를 저장하는 구조입니다.

각 키(key)는 고유하며, 이를 통해 해당 값(value)에 빠르게 접근할 수 있는 자료 구조입니다.

# 동작 원리

대부분의 딕셔너리는 [해시 테이블(hash table)](/posts/%ED%95%B4%EC%8B%9C-%ED%85%8C%EC%9D%B4%EB%B8%94(hash-table)%EA%B3%BC-%EA%B0%9C%EB%B0%A9-%EC%A3%BC%EC%86%8C%EB%B2%95(open-addressing),-%EC%B2%B4%EC%9D%B4%EB%8B%9D(chaining)/){: target="_blank"}을 사용하여 구현됩니다.

해시 테이블은 키(key)를 해시 함수(hash function)에 통과시켜 해시 값(hash value)을 생성하고, 이 해시 값을 인덱스(Index)로 사용하여 데이터를 저장합니다.

- **해시 함수:** 해시 함수는 키를 입력받아 고정된 크기의 해시 값을 출력하는 함수입니다. 이 함수는 같은 키에 대해 항상 동일한 해시 값을 반환하며, 다른 키에 대해서는 가능한 한 충돌을 최소화하도록 다른 해시 값을 반환하도록 설계됩니다.
- **해시 충돌:** 서로 다른 키가 동일한 해시 값을 생성하는 경우, 이를 해시 충돌(hash collision)이라고 합니다. 해시 충돌을 해결하기 위한 대표적인 방법으로는 체이닝(chaining)과 개방 주소법(open addressing)이 있습니다.

# 특징

- **빠른 조회 속도:** 평균적으로 $O(1)$의 시간 복잡도로 <span class="font_highlight">키(key)를 통해 값(value)을 조회</span>할 수 있습니다.
- **순서 없음:** 딕셔너리는 <span class="font_highlight">**순서를 보장하지 않습니다**</span>. 다만, 일부 언어에서는 삽입 순서를 유지하는 형태의 딕셔너리를 지원하기도 합니다.
- **고유 키:** 딕셔너리의 <span class="font_highlight">키(key)는 **고유**해야 합니다</span>. 만약, 중복된 키(key)를 사용하면 마지막으로 삽입한 값(value)으로 덮어씌워지게 됩니다.

# 장단점

장단점의 경우, 프로그래밍 언어에 따라, 구현하는 구체적인 자료구조에 따라 다를 수 있습니다.

## 장점

- **빠른 접근 속도:** 해시 테이블을 사용하여 평균적으로 $O(1)$의 시간 복잡도로 데이터를 조회할 수 있습니다.
- **유연성:** 키(key)-값(value)의 쌍으로 다양한 형태의 데이터를 저장할 수 있습니다.

## 단점

- **메모리 사용량:** 해시 테이블은 성능을 위해 추가적인 메모리를 사용합니다.
- **해시 충돌:** 해시 충돌이 발생하면 성능이 저하될 수 있으며, 충돌 해결을 위한 추가적인 비용이 발생합니다.

---

# 참고

[해시 테이블(Hash Table)과 개방 주소법(Open Addressing), 체이닝(Chaining)](/posts/%ED%95%B4%EC%8B%9C-%ED%85%8C%EC%9D%B4%EB%B8%94(hash-table)%EA%B3%BC-%EA%B0%9C%EB%B0%A9-%EC%A3%BC%EC%86%8C%EB%B2%95(open-addressing),-%EC%B2%B4%EC%9D%B4%EB%8B%9D(chaining)/){: }