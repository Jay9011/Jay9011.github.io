---
title: STL - 알고리즘 (Algorithm)
date: 2023-11-21 19:34:59 +0900
categories:
    - 컴퓨터 과학
    - 자료구조
tags:
    - c
    - 알고리즘
    - 자료구조
    - 이터레이터
math: false
type: DataStructure
keywords:
    - Algorithm
    - STL
    - 알고리즘
---

STL 알고리즘(Algorithm)은 **데이터를 처리하고 조작하는 데 사용되는 함수 템플릿의 집합**입니다.

C++ STL의 **`<algorithm>`** 헤더 파일에는 다양한 알고리즘 함수들이 포함되어 있습니다.

이 알고리즘들의 일부를 나름대로 카테고리별로 나누어 표로 정리해 봤습니다.

# 정렬

| 알고리즘 | 설명 | 사용 예 |
| --- | --- | --- |
| sort | 범용 정렬 알고리즘 | sort(begin, end, pred) |
| stable_sort | 안정(순서 유지) 정렬 | stable_sort(begin, end, pred) |
| partial_sort | 부분적 정렬 | partial_sort(begin, middle, end, pred) |
| nth_element | n번째 요소를 찾아 정렬 | nth_element(begin, nth, end, pred) |

# 검색

| 알고리즘 | 설명 | 사용 예 |
| --- | --- | --- |
| find | 값 검색 | find(begin, end, value) |
| find_if | 조건에 맞는 값 검색 | find_if(begin, end, pred) |
| find_if_not | 조건에 맞지 않는 첫 번째 값 검색 | find_if_not(begin, end, pred) |
| binary_search | 이진 검색 | binary_search(begin, end, value, pred) |

# 변환

| 알고리즘 | 설명 | 사용 예 |
| --- | --- | --- |
| transform | 요소에 함수 적용 | transform(begin, end, dest, func) |
| replace | 값 변경 | replace(begin, end, old, new) |
| replace_if | 조건에 따른 값 변경 | replace_if(begin, end, pred, new) |

# 순열

| 알고리즘 | 설명 | 사용 예 |
| --- | --- | --- |
| next_permutation | 다음 순열 생성 | next_permutation(begin, end, pred) |
| prev_permutation | 이전 순열 생성 | prev_permutation(begin, end, pred) |

# 수치 연산

수치 연산 관련 알고리즘은 **`<numeric>`** 헤더 파일에 존재하지만, 이터레이터를 사용하기 때문에 알고리즘 부분에 포함해서 작성했습니다.

| 알고리즘 | 설명 | 사용 예 |
| --- | --- | --- |
| accumulate | 요소들의 합 계산 | accumulate(begin, end, init) |
| inner_product | 두 범위의 내적 계산 | inner_product(begin1, end1, begin2, init) |
| adjacent_difference | 인접 요소 간의 차이 계산 | adjacent_difference(begin, end, result) |
| partial_sum | 부분 합 계산 | partial_sum(begin, end, result) |

# 집합 연산

| 알고리즘 | 설명 | 사용 예 |
| --- | --- | --- |
| set_union | 합집합 | set_union(begin1, end1, begin2, end2, result) |
| set_intersection | 교집합 | set_intersection(begin1, end1, begin2, end2, result) |
| set_difference | 차집합 | set_difference(begin1, end1, begin2, end2, result) |
| set_symmetric_difference | 대칭 차집합 | set_symmetric_difference(begin1, end1, begin2, end2, result) |

# 기타 알고리즘

기타 알고리즘은 `<algorithm>` 뿐만이 아니라 `<xutility>`나 `<xmemory>`등에 존재하는 것들도 포함되어 있지만, 이들은 `<algorithm>` 내부 구현에 포함되어 있으므로 **`<algorithm>`** 헤더 파일 만으로 사용할 수 있습니다.

| 알고리즘 | 설명 | 사용 예 |
| --- | --- | --- |
| for_each | 각 요소에 함수 적용 | for_each(begin, end, func) |
| max_element | 최대값 요소 찾기 | max_element(begin, end) |
| min_element | 최소값 요소 찾기 | min_element(begin, end) |
| count | 값의 개수 세기 | count(begin, end, value) |
| count_if | 조건을 만족하는 요소 개수 세기 | count_if(begin, end, pred) |
| remove | 값 제거 | remove(begin, end, value) |
| remove_if | 조건에 맞는 요소 제거 | remove_if(begin, end, pred) |
| swap | swap 은 두 변수의 값을 교환합니다. <br>이 함수는 `<utility>` 헤더나, 일부 컨테이너 타입에 대한 특수화된 버전도 있을 수 있습니다. <br>swap의 정확한 사용 방법은 변수의 타입과 상황에 따라 달라질 수 있으므로, <br>필요에 따라 IDE의 인텔리센스나 문서를 참조하세요. | IntelliSense 참조 |

---

# 참고

[STL - Standard Template Library 란?](/posts/stl-standard-template-library-%EB%9E%80/)

[STL - 컨테이너 (Container)](/posts/stl-%EC%BB%A8%ED%85%8C%EC%9D%B4%EB%84%88-(container)/)

[STL - 반복자 (Iterator) (with.C++)](/posts/stl-%EB%B0%98%EB%B3%B5%EC%9E%90-(iterator)-(with.c++)/)