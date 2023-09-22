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
특히 OS의 커널에서는 더욱 그렇다. 문제가 생기는 순간 머신 전체에 영향을 끼친다. Pintos는 몇가지의 원시적인 동기화 기법을 제공한다.


## 인터럽트 비활성화
동기화를 수행하는 날것의 방법은 인터럽트를 비활성화 하는 것이다. 잠깐이마나 CPU가 인터럽트에 반응하는걸 막을 수 있지만, 다른 어떠한 실행중인 스레드도 preempt 할 수 없다. (우선순위를 받아 실행되는 것)
왜냐하면 스레드의 preemption은 타이머의 인터럽트로 인해 수행되기 때문이다. 인터럽트가 평소처럼 잘 작동하고 있다면 실행중인 스레드들은 서로 preempt하면서 잘 작동할 수 있다. (두 C statement 사이나 실행중인 것 사이에서 라고 언급하는데 조사가 필요함)

Pintos에서 preemptible kernel이라는 뜻은 커널 스레드는 언제든지 preempt(우선순위를 받아 바로 실행될 수 있다)할 수 있다. 전통적인 Unix system은 non-preemptible 한데 커널 스레드라도 스케줄러가 명시적으로 우선순위를 부여해야만 호출 될 수 있다는 뜻이다. (유저 프로그램은 양쪽 모두에서 언제든지 preempt 될 수 있다.) 생각한것처럼 preemptible kernel은 더욱 명확한 동기화가 필요하다.
(@@ non-preemptible 은 커널스레드, 유저스레드 상관없이 스케줄링만 하면 되지만, premptible은 커널스레드에 대해서 특수한 취급을 해야한다.)

당신은 인터럽트 상태에 대해 알아야 한다. 아래의 섹션에 따라 다른 동기화 preempt를 사용해야 한다. 인터럽트를 비활성화 하는 주 이유는 커널 스레드를 외부 인터럽트 핸들러와 동기화 시키기 위함인데, 절대 휴식하지 않기 때문에 (cannot sleep) 대부분의 다른 동기화 형태를 취할 수 없기 때문이다. (@@ 인터럽트를 비활성화 시키고 커널 스레드를 busy wait 상태에 두면 다른작업은 아무것도 할 수 없다. )

몇몇 외부 인터럽트는 연기될 수 없다. 심지어 인터럽트 비활성화로도! 이러한 인터럽트는 non-maskable interrupts(NMIs)라고 하며 주로 긴급한 상황에서만 사용된다.
예를 들어 컴퓨터가 불에 붙으면(미친듯이 일하고 있으면) Pintos는 non-maskable interrupts를 handle하지 않는다. (@ qemu 위에서 pintos가 작동하는데 긴급상황이 발생해 외부 인터럽트가 들어오면 pintos 가 이를 처리하지 않고 받아들인다는 뜻 같다.)

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

세마포어가 **단 한번** 일어날 이벤트를 기다릴때 0으로 초기화된다. 예를 들어 스레드 A가 다른 스레드 B를 시작시키고  B의 some activity가 끝나기를 기다리기를 원한다고 해보자.  B의 작업이 끝이나면 signal을 줄 것이다. A는 세마포어를 만들어서 0으로 초기화 시키고 B에게 전달한다. 그리고 세마포어를 **다운** 시킨다. 스레드 B가 작업을 종료하면 세마포어를 **업** 시킨다. 이 작업들은 A가 먼저 세마포어를 **다운** 하든 B가 먼저 세마포어를 **업** 하든 상관없이 작동한다. (@ 아래의 코드를 보면 A가 먼저 실행되든, B가 먼저 실행되든 최종 실행 순서는 B -> A 가 된다.)

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

세마포어는 내부적으로 interrupt의 비활성화와 (see [Disabling Interrupts](https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html#Disabling%20Interrupts)) 스레드의 blocking & unblocking (`thread_block()` and `thread_unblock()`) 을 위해서 제작되었다.  각각의 세마포어는 대기하고있는 스레드의 리스트를 유지하며, 연결 리스트를 사용한다. 연결리스트는`lib/kernel/list.c` 에 구현되어 있다.

## Locks 잠금

Lock은 세마포어의 초기값이 1인 것과 비슷하다. (see [Semaphores](https://casys-kaist.github.io/pintos-kaist/appendix/synchronization.html#Semaphores)). "up"은 release, "down"은 acquire을 호출한 것과 같다.

세마포어와 비교해서 lock은 한가지 규제를 추가로 한다.: 오직 thread만 lock을 acquire 할 수 있다. (lock의 소유자 만 가능하다.) 또한 release가 가능하다. 만약 이러한 규제가 문제가 된다면 lock 대신에 semaphore가 사용되어야 한다는 좋은 사인(sign, 부호)이다. 

pintos에서 lock은 재귀적으로 작동하지 않는다. 이는 스레드가 holding하고 있는 lock을 acquire 할 경우 error가 발생한다. Lock type과 functions는 `include/threads/synch.h` 에 정의되어 있다.

---

```
struct lock;
```

> Represents a lock.

---

```
void lock_init (struct lock *lock);
```

> Initializes lock as a new lock. The lock is not initially owned by any thread.

---

```
void lock_acquire (struct lock *lock);
```

> Acquires lock for the current thread, first waiting for any current owner to release it if necessary.

---

```
bool lock_try_acquire (struct lock *lock);
```

> Tries to acquire lock for use by the current thread, without waiting. Returns true if successful, false if the lock is already owned. Calling this function in a tight loop is a bad idea because it wastes CPU time, so use `lock_acquire()` instead.

---

```
void lock_release (struct lock *lock);
```

> Releases lock, which the current thread must own.

---

```
bool lock_held_by_current_thread (const struct lock *lock):
```

> Returns true if the running thread owns lock, false otherwise. There is no function to test whether an arbitrary thread owns a lock, because the answer could change before the caller could act on it.


## Monitors 모니터

모니터는 세마포어나 lock 을 이용한 동기화 형태보다 higher-level 이다. 모디터는 동기화된 data와 lock( monitor lock), 그리고 하나 또는 그 이상의 조건 변수(Condition variables)로 으로 구성되어 있다. 
protected data에 접근하기 전에 스레드는 1. monitor lock을 acquire한다. 이 행위는 "in the monitor"라고 일컫어진다. 모니터에 있는 동안은 스레드는 proteced data를 다루게 되는데 자유롭게 살펴보거나(examine) 수정(modify)할 수 있다. protected data에 접근이 끝나면 monitor lock을 해제한다.

Condition variables (조건 변수) 들은 true가 될 때 까지 모니터안의 코드를 기다리도록 허용한다. 각각의조건 변수들은 추상적인 조건에 연관되어 있는데, 예를 들자면 "어떠한 데이터가 작업 진행을 위해 도착했다.", "사용자의 key stroke가 전달된 지 10초가 지났다" 등이 있다. 모니터에 진입한 코드가 condition == true가 되는걸 기다릴 필요가 있다면 release lock & signal the condition이 될때까지 기다린다. 만약, 반대로 코드가 conditions variables를 true로 만든다면 기다리고 있는 모든 코드들에게 "signal을 보내거나", "broadcast" 한다.

모니터의 이론적 프레임워크는 C.A.R.Hoare 이다. 실제 사용은 Mesa operation system 에서 구체화 되었다. Condition variables types & functions 는 `include/threads/synch.h` 에 선언되어 있다.

---

```
struct condition;
```

> Represents a condition variable.

---

```
void cond_init (struct condition *cond);
```

> Initializes cond as a new condition variable.

---

```
void cond_wait (struct condition *cond, struct lock *lock);
```

> Atomically releases lock (the monitor lock) and waits for cond to be signaled by some other piece of code. After cond is signaled, reacquires lock before returning. lock must be held before calling this function. Sending a signal and waking up from a wait are not an atomic operation. Thus, typically `cond_wait()`'s caller must recheck the condition after the wait completes and, if necessary, wait again. See the next section for an example.

---

```
void cond_signal (struct condition *cond, struct lock *lock);
```

> If any threads are waiting on cond (protected by monitor lock lock), then this function wakes up one of them. If no threads are waiting, returns without performing any action. lock must be held before calling this function.

---

```
void cond_broadcast (struct condition *cond, struct lock *lock);
```

> Wakes up all threads, if any, waiting on cond (protected by monitor lock lock). lock must be held before calling this function.