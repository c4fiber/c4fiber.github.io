---
title: Synchronization
aliases: 
tags:
  - pintos
date created: 2023-09-21 17:42
---
https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html 파트 정리

# 동기화

쓰레드 사이의 자원 공유가 조심스럽게 이루어지지 않으면(controlled fasion) 난리난다.
특히 OS의 커널에서는 더욱 그렇다.


## 인터럽트 비활성화
동기화를 수행하는 날것의 방법은 인터럽트를 비활성화 하는 것이다. 잠깐이마나 CPU가 인터럽트에 반응하는걸 막을 수 있지만, 다른 어떠한 실행중인 스레드도 preempt 할 수 없다. (우선순위를 받아 실행되는 것)
왜냐하면 스레드의 preemption은 타이머의 인터럽트로 인해 수행되기 때문이다. 인터럽트가 평소처럼 잘 작동하고 있다면 실행중인 스레드들은 서로 preempt하면서 잘 작동할 수 있다. (두 C statement 사이나 실행중인 것 사이에서 라고 언급하는데 조사가 필요함)

Pintos에서 preemptible kernel이라는 뜻은 커널 스레드는 언제든지 preempt(우선순위를 받아 바로 실행될 수 있다)할 수 있다. 전통적인 Unix system는 non-preemptible 한데 스케줄러가 명시적으로 우선순위를 부여해야만 호출 될 수 있다는 뜻이다. (유저 프로그램은 양쪽 모두에서 언제든지 preempt 될 수 있다.) 생각한것처럼 preemptible kernel은 더욱 명확한 동기화가 필요하다.

인터럽트 상태에 대해 알아야 한다. 아래의 섹션에 따라 다른 동기화 preempt를 사용해야 한다. 인터럽트를 비활성화 하는 주 이유는 커널 스레드를 외부 인터럽트 핸들러와 동기화 시키기 위함인데, 절대 휴식하지 않기 때문에 (cannot sleep) 대부분의 다른 동기화 형태를 취할 수 없기 때문이다.

몇몇 외부 인터럽트는 연기될 수 없다. 심지어 인터럽트 비활성화로도! 이러한 인터럽트는 non-maskable interrupts(NMIs)라고 하며 주로 긴급한 상황에서만 사용된다.
예를 들어 컴퓨터가 불에 붙으면 Pintos는 non-maskable interrupts를 handle하지 않는다.

인터럽트 활성화&비활성화 타입과 함수는 `include/threads/interrupt.h` 에 있다.

![](/assets/images/Pasted%20image%2020230922145136.png)

## 세마포어 Semaphores

부호가 없는 정수이며 두 operation의 원자성을 해치지 않게 다룬다.

Down, P: 세마포어의 값이 positive가 될 때 까지 기다린다. positive가 되면 값을 하나 감소시킨다.
Up, V: 값을 증가시키고 대기하는 하나의 스레드를 깨운다.

세마포어는 0으로 초기화 되며 