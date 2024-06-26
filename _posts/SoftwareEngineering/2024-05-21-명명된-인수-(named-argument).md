---
title: 명명된 인수 (Named Argument)
date: 2024-05-21 16:22:58 +0900
categories:
  - 컴퓨터 공학
  - 소프트웨어 공학
tags:
  - 변수
  - 소프트웨어 공학
  - 유연성
  - 함수
math: false
type: SoftwareEngineering
keywords:
  - 명명된 인수
  - Named Argument
---

명명된 인수(named argument)란, 프로그래밍에서 함수 호출 시 인수를 지정할 때 사용하는 방법 중 하나로, <span class="font_highlight">**인수의 이름을 명시적으로 지정**하여 값을 전달하는 것</span>을 말합니다.

이 방법은 코드의 가독성을 높이는 동시에 인수의 순서에 구애받지 않고 값을 전달할 수 있는 유연성을 제공합니다.

# 특징

1. **기본값과 함께 사용**: 함수 정의 시 기본값을 지정할 수 있는데, 명명된 인수와 함께 사용하면 <span class="font_highlight">기본값을 유지하면서 특정 인수만 변경</span>할 수 있습니다.
2. **순서 무관**: 인수를 전달할 때 <span class="font_highlight">순서를 반드시 지킬 필요가 없습니다</span>.

# 장점

1. **가독성 향상**: 함수 호출 시 어떤 매개변수에 어떤 값이 들어가는지 명확하게 알 수 있어 코드의 이해와 유지보수가 용이합니다.
2. **유연성**: 인수의 순서에 구애받지 않으므로, 많은 매개변수를 가진 함수에서 특히 유리합니다.
3. **오류 방지**: 인수를 잘못된 순서로 전달하는 일이 없어, 관련 오류를 예방할 수 있습니다.

# 예제

다음 예제는 명명된 인수를 사용하지 않을 때 불편함과 명명된 인수를 사용할 때에 대한 예제입니다.

```python
def configure_server(ip, port=8080, use_ssl=True, timeout=60):
    print(f"IP: {ip}, Port: {port}, SSL: {use_ssl}, Timeout: {timeout}")
```

`configure_server` 함수는 IP 주소, 포트, SSL 사용 여부, 타임아웃을 설정하는 매개변수를 가집니다.

위 함수를 호출할 때, 명명된 인수를 사용하지 않으면 다음과 같이 모든 인수를 순서대로 전달해야 합니다.
<br>
특히, 중간에 있는 인수만 변경하려고 하더라도 앞에 있는 인수들도 모두 명시해야 합니다.

```python
configure_server("192.168.1.1", 8080, True, 30)
```

명명된 인수를 사용하면 변경하고자 하는 인수만 지정하면 됩니다. 나머지 인수는 기본값을 사용합니다.

```python
configure_server("192.168.1.1", timeout=30)
```

이와 같이 명명된 인수를 사용하면 함수 호출 시 인수의 가독성과 유연성이 크게 향상되며, 코드의 유지보수가 더 쉬워집니다.