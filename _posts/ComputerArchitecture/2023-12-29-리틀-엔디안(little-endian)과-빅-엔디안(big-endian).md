---
title: 리틀 엔디안(Little Endian)과 빅 엔디안(Big Endian)
date: 2023-12-29 19:30:59 +0900
categories:
  - 컴퓨터 공학
  - 컴퓨터 구조
tags:
  - 컴퓨터 구조
  - 메모리
math: false
type: ComputerArchitecture
keywords:
  - Big Endian
  - Little Endian
  - 리틀 엔디안
  - 빅 엔디안
  - 메모리
---

<span class="keyword">**리틀 엔디안(Little Endian)**</span>과 <span class="keyword">**빅 엔디안(Big Endian)**</span>은 컴퓨터가 <span class="font_highlight">**메모리에 데이터를 저장하는 방법**</span> 두 가지를 말합니다.

컴퓨터가 메모리에 데이터를 저장할 때, <span class="important">**바이트(byte)**</span> 단위로 나누어 저장하게 됩니다.

하지만 데이터는 여러 바이트(int의 경우 4byte)로 구성되어 있기 때문에, 이 데이터를 각 주소에 나누어 저장하게 되는데, 이때 <span class="font_highlight">**저장되는 바이트의 순서에 따라**</span> 리틀 엔디안(Little Endian)과 빅 엔디안(Big Endian)으로 나뉘게 됩니다.

# 빅 엔디안 (Big Endian)

빅 엔디안 방식은 <span class="font_highlight">**데이터의 최상위 바이트(Most Significant Byte, MSB)**</span>가 가장 먼저 저장되는 방식입니다.

즉, 데이터의 최상위 바이트가 메모리의 가장 낮은 주소에 저장됩니다.

예를 들어, `0x12345678`을 저장한다고 하면 아래와 같은 방식으로 저장됩니다.

```cpp
// 주소:0x00 0x01 0x02 0x03
// 값:  0x12 0x34 0x56 0x78
```

![빅 엔디안](https://i.postimg.cc/jqfQyXmm/Little-Endian-Big-Endian-Big-Endian-Img.webp){: w="250" h="174"}

이 방식은 디버깅시 우리가 평소에 사용하는 선형 방식으로 볼 수 있으며, 읽고 바로 이해할 수 있습니다.

SPARC을 포함한 대부분의 RISC CPU 계열에서 이 방식으로 데이터를 저장합니다.


# 리틀 엔디안 (Little Endian)

리틀 엔디안 방식은 <span class="font_highlight">**데이터의 최하위 바이트(Least Significant Byte, LSB)**</span>가 가장 먼저 저장되는 방식입니다.

즉, 데이터의 최하위 바이트가 메모리의 가장 낮은 주소에 저장됩니다.

예를 들어, `0x12345678`을 저장한다고 하면 아래와 같은 방식으로 저장됩니다.

```cpp
// 주소:0x00 0x01 0x02 0x03
// 값:  0x78 0x56 0x34 0x12
```

![리틀 엔디안](https://i.postimg.cc/nrq4ZFqp/Little-Endian-Big-Endian-Little-Endian-Img.webp){: w="250" h="164"}

이 방식은 대부분의 인텔 CPU 계열에서 사용하는 방식입니다.

실제로 visual studio로 디버깅을 하게 되면, 리틀 엔디안 방식으로 저장되어 있는 것을 확인 할 수 있습니다.

![리틀 엔디안 디버깅](https://i.postimg.cc/NM36W99d/Little-Endian-Big-Endian-Little-Endian.webp)