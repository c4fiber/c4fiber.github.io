---
title: Synchronization
aliases: 
tags:
  - pintos
date created: 2023-09-21 17:42
---
https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html 파트 정리

@로 시작하는 문장은 내가 이해를 했는지 기록하기 위한 내용이다. 본문과는 전혀 관련 없으니 주의해서 읽어주길 바란다.

# 동기화 Synchronization

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

---

```
enum intr_level;
```

> One of INTR_OFF or INTR_ON, denoting that interrupts are disabled or enabled, resepectively.

---

```
enum intr_level intr_get_level (void)
```

> Returns the current interrupt state.

---

```
enum intr_level intr_set_level (enum intr_level level);
```

> Turns interrupts on or off according to level. Returns the previous interrupt state.

---

```
enum intr_level intr_enable (void);
```

> Turns interrupts on. Returns the previos interrupt state.

---

```
enum intr_level intr_disable (void);
```

> Turns interrupts off. Returns the previous interrupt state.


## 세마포어 Semaphores

부호가 없는 정수이며 두 operation의 원자성을 해치지 않게 다룬다.

Down, P: 세마포어의 값이 positive가 될 때 까지 기다린다. positive가 되면 값을 하나 감소시킨다.
Up, V: 값을 증가시키고 대기하는 하나의 스레드를 깨운다.

세마포어는 0으로 초기화 되며 **단 한번** 일어날 이벤트를 기다린다. 예를 들어 스레드 A가 다른 스레드 B를 시작하게 한다고 가정해보자. 다른 스레드 B가 A의 some activity가 끝나기를 기다리기를 원할 것이고 끝이나면 signal을 줄 것이다. A는 세마포어를 만들어서 0으로 초기화 시키고 B에게 전달한다. 그리고 세마포어를 **다운** 시킨다. (@나는 값이 -1이 될 것이라 예상한다. 이를 통해 세마포어가 >= 0 일때 작업을 수행할 수 있다고 생각한다.) 스레드 B가 작업을 종료하면 세마포어를 **업** 시킨다. 이 작업들은 A가 먼저 세마포어를 **다운** 하든 B가 먼저 세마포어를 **업** 하든 상관없이 작동한다.

```c
struct semaphore sema;

/* Thread A */
void threadA (void) {
    sema_down (&sema);
}

/* Thread B */
void threadB (void) {
    sema_up (&sema);
}

/* main function */
void main (void) {
    sema_init (&sema, 0);
    thread_create ("threadA", PRI_MIN, threadA, NULL);
    thread_create ("threadB", PRI_MIN, threadB, NULL);
}
```

이 예제에서는 스레드 A가 `sema_down()` 에서 작업을 멈춘다. 언제까지? 스레드 B가 `sema_up()` 을 호출 할 때까지. 

세마포어를 자원의 접근을 통제하기 위해 사용할 때는 전형적으로 1로 초기화 시킨다. 코드 블록이 자원에 접근하기 전에 세마포어를 **다운** 시키고 작업이 끝나면 **업** 시킨다. 이러한 경우에는 아래에 설명한 방법이 좀 더 적절하다.

세마포어는 또한 1 보다 더 큰값으로 초기화 될 수 있다. 이러한 경우는 잘 사용되지는 않는다. 세마포어는 Edsger Dijkstra에 의해 발명되었고 처음 사용된 곳은 THE operating system 이다. 핀토스의 세마포어 타입은 `include/threads/synch.h` 에 선언되어 있다.


---

```
struct semaphore;
```

> Represents a semaphore.

---

```
void sema_init (struct semaphore *sema, unsigned value);
```

> Initializes sema as a new semaphore with the given initial value.

---

```
void sema_down (struct semaphore *sema);
```

Executes the "down" or "P" operation on sema, waiting for its value to become positive and then decrementing it by one.

---

```
bool sema_try_down (struct semaphore *sema);
```

> Tries to execute the "down" or "P" operation on sema, without waiting. Returns true if sema was successfully decremented, or false if it was already zero and thus could not be decremented without waiting. Calling this function in a tight loop wastes CPU time, so use `sema_down()` or find a different approach instead.

---

```
void sema_up (struct semaphore *sema);
```

> Executes the "up" or "V" operation on sema, incrementing its value. If any threads are waiting on sema, wakes one of them up. Unlike most synchronization primitives, `sema_up()` may be called inside an external interrupt handler.