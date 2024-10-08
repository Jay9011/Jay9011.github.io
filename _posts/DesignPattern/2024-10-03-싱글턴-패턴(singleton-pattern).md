---
title: 싱글턴 패턴(Singleton Pattern)
date: 2024-10-03 19:11:10 +0900
categories:
  - 컴퓨터 공학
  - 디자인 패턴
tags:
  - 객체 지향
  - 디자인 패턴
  - 메모리
  - 재사용성
  - 전역
  - 편의성
  - 효율성
math: false
fmContentType: DesignPattern
keywords:
  - Singleton
  - 객체 지향
  - 싱글턴
  - 디자인 패턴
---

**싱글턴 패턴(Singleton Pattern)**은 객체 지향 프로그래밍에서 매우 중요한 디자인 패턴 중 하나로 <span class="font_highlight">클래스의 **인스턴스**를 **하나**만 생성</span>하고, <span class="font_highlight">그 인스턴스에 접근할 수 있는 **전역적인 접근**점을 제공</span>하는 것을 목적으로 합니다.

이를 통해 시스템 내에서 <span class="font_highlight">해당 클래스의 **인스턴스**가 **단 하나**만 존재하도록 보장하는 패턴</span>입니다.

# 핵심 개념

싱글턴 패턴의 기본적인 핵심 개념은 다음과 같습니다.

- **유일한 인스턴스**: 클래스의 인스턴스가 하나만 생성됩니다.
- **전역 접근**: 생성된 인스턴스에 어디서든 접근할 수 있습니다.

## 사용 이유

싱글턴 패턴은 주로 설정 정보나 로그 관리 등, 전역적으로 동일한 자원을 가지며 같은 상태를 유지하고 싶을 때, 주로 사용하게 됩니다.

설정 정보, 로그 관리, 데이터베이스 연결, 파일 시스템 접근 등 한 번에 하나의 인스턴스만을 필요로 할 때 사용합니다.

# 싱글턴 패턴의 장단점

## 장점

1. **일관된 접근**: 유일한 인스턴스를 사용하므로 **상태 관리**가 용이합니다.
2. **자원 절약**: 인스턴스가 하나만 생성되므로 **메모리와 자원을 절약**할 수 있습니다.
3. **글로벌 접근**: 어디서든 인스턴스에 **접근**할 수 있어 코드의 가독성이 높아집니다.

## 단점

1. **테스트 어려움**: 전역 상태를 사용하므로 테스트가 어려울 수 있습니다.
2. **의존성 문제**: 코드가 싱글턴 인스턴스에 강하게 의존하게 되어 유연성이 떨어질 수 있습니다.
3. **멀티스레드 문제**: 멀티스레드 환경에서 동기화 문제를 신경 써야 합니다.

# 구현 방법

C++로 싱글턴 패턴을 구현하는 간단한 방법은 다음과 같습니다.

```cpp
class Singleton {
private:
    // 생성자를 private으로 설정하여 외부에서 임의로 인스턴스 생성을 막음
    Singleton() {}

    // 소멸자도 private으로 설정
    ~Singleton() {}

    // 복사 생성자와 대입 연산자를 삭제하여 복사 방지
    Singleton(const Singleton&) = delete;
    Singleton& operator=(const Singleton&) = delete;

public:
    // 유일한 인스턴스를 반환하는 정적 메서드 정의
    static Singleton& getInstance() {
        static Singleton instance;
        return instance;
    }

    void doSomething() {
    }
};
```

싱글턴 클래스의 기본은 생성자를 `private`로 설정하여 외부에서 임의로 인스턴스를 생성하지 못하도록 하는 것입니다.

이때, 보통 클래스를 설계했다면, 단순한 함수만 사용하는 클래스가 아니라면 인스턴스를 생성해서 사용하게 됩니다.

그러나 생성자를 사용할 수 없으므로 다른 방법을 통해 인스턴스를 생성해야 하는데, 이때, <span class="font_highlight">**정적 메서드**를 사용해서 **클래스 인스턴스를 생성**</span>할 수 있도록 해줄 수 있습니다.

클래스의 정적 메서드 안에서 **정적 지역 변수 초기화**를 사용하기 때문에 <span class="font_highlight">딱 한 번 인스턴스를 생성하게 되며, 프로그램이 종료 될 때까지 유지</span>됩니다.

또한, 복사 생성자와 대입 연산자를 `delete` 키워드를 사용하여 삭제했으므로 싱글턴 인스턴스가 복사되는 것도 보장할 수 있습니다.

```cpp
int main() {
    // 싱글턴 인스턴스에 접근하여 메서드 호출
    Singleton& singleton = Singleton::getInstance();
    singleton.doSomething();
}
```

이제, 해당 클래스의 정적 메서드를 호출하면, 클래스의 인스턴스를 반환 받게 되고, 이후, 해당 인스턴스에 접근해 여러 공개 메서드들을 사용할 수 있게 됩니다.

# 멀티 쓰레드 문제

멀티 쓰레드 환경에서 싱글턴 패턴을 구현할 때 주의해야 할 점은 동시성 문제입니다.

여러 쓰레드가 동시에 싱글턴 인스턴스를 생성하려고 할 때, 그리고 하나의 싱글턴 인스턴스에서 공유 자원을 사용하려고 할 때 문제가 발생할 수 있습니다.

기본적인 예시로 멀티 쓰레드 환경에서 싱글턴 패턴을 잘못 구현하면, 여러 쓰레드가 동시에 싱글턴 인스턴스를 생성하려고 할 때, 두 개 이상의 인스턴스가 생성될 수 있는 위험이 있습니다.

이를 <span class="keyword">**경쟁 조건(Race Condition)**</span>이라고 합니다. 이러한 문제를 해결하기 위해 <span class="important">**동기화(Synchronization)**</span>가 필요합니다.

> 물론, 위 예제에서는 C++11 이후 정적 변수의 초기화가 쓰레드 안전성을 보장하기 때문에 문제는 없을 수 있습니다. <br>
> 하지만, 다른 언어를 사용하거나 이전 버전의 C++에서는 다른 방법이 필요할 수 있습니다.
>
{: .prompt-info}

## 동기화(Synchronization)란?

동기화는 여러 쓰레드가 동시에 접근할 수 있는 공유 자원에 대해 한 번에 하나의 쓰레드만 접근할 수 있도록 제어하는 방법입니다. 이를 통해 경쟁 조건을 방지할 수 있습니다.

이때, 주로 사용하는 방법에는 뮤텍스(Mutex)라는게 있습니다.

## 뮤텍스(Mutex)란?

**뮤텍스(Mutex)**는 "**Mutual Exclusion**"의 약자로, <span class="font_highlight">여러 쓰레드가 동시에 공유 자원에 접근하지 못하도록 **상호 배제**를 보장하는 동기화 도구</span>입니다.

뮤텍스를 사용하면 한 번에 하나의 쓰레드만 특정 코드 블록을 실행할 수 있습니다.

```java
import java.util.concurrent.locks.ReentrantLock;

class Singleton {
    private static Singleton instance = null;
    private static final ReentrantLock lock = new ReentrantLock();

    private Singleton() { }

    public static Singleton getInstance() {
        lock.lock();
        try {
            if (instance == null) {
                instance = new Singleton();
            }
        } finally {
            lock.unlock();
        }
        return instance;
    }

    public void doSomething() {
    }
}
```

예를 들어, 위 예제는 Java의 ReentrantLock을 이용하여 뮤텍스 기법을 사용한 것입니다.

`lock`으로 구역을 잠그고 try-finally 블록에서 `unlock` 으로 해제합니다.

## 더블 체크 락킹(Double-Checked Locking)

또한, 더블 체크 락킹(Double-Checked Locking)이라는 방법도 있는데, 더블 체크 락킹은 성능을 최적화하면서도 쓰레드 안전성을 보장하는 방법입니다. (완전한 쓰레드 안전성을 보장하지는 않습니다.)

예를 들어, 인스턴스를 생성하려고 할 때, 인스턴스가 이미 생성되었는지 **두 번 확인**하여 불필요한 락을 피하는 동시에 인스턴스를 하나만 생성하도록 유도하는 방법입니다.

# 싱글턴 패턴 사용시 주의사항

1. **필요한 경우에만 사용**: 싱글턴 패턴은 접근이 편리하다는 장점이 있습니다. 그래서인지 자주 사용하려고 하는 경향이 보이기도 하는데, 싱글턴 패턴은 클래스의 인스턴스가 **단 하나만 필요할 때만 사용**해야 합니다.
2. **동기화 고려**: 또한, 멀티 쓰레드 환경에서 안전하게 사용할 수 있도록 동기화를 고려해야 합니다. 예를 들어, 상태를 공유하려고 할 때, 여러 쓰레드에서 동시에 상태에 접근하고 바꾸려고 하는 것에 대해 생각해 보셔야 합니다.

싱글턴 패턴은 전역적인 접근을 통해 공유 자원 혹은 상태에 쉽게 접근할 수 있다는 장점이 있습니다. 하지만, 단순히 클래스를 전역적으로 만들고, 그 안에서 상태를 바로 관리하는 형태는 코드의 결합도를 높여 코드의 유지보수성과 확장성을 저하시킬 수 있습니다.

싱글턴 패턴을 사용하기 전, 의존성 주입(Dependency Injection)이나 팩토리 패턴(Factory Pattern) 등으로 해결할 수 없는지, 혹은 싱글턴 패턴과 의존성 주입을 결합해서 사용할 수 있는지 등을 고려해서 최대한 코드 결합도를 낮추고 코드의 유지보수성과 확장성을 높이는 방향에 대해 생각해 보시는 것도 좋을 것 같습니다.