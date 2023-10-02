---
title: Interrupt
aliases:
  - pintos-interrupt
tags: 
date created: 2023-09-25 21:30
---
pintOS를 공부하면서 Interrupt에 대해 작성해봤다.

## 인터럽트의 privilege


DPL: Descriptor Privilege Level (2bit)

![](/assets/images/Pasted%20image%2020230925221748.png)


## 인터럽트 호출시점

`inter_entry` 가 수행되면 현재 레지스터 상태를 **실행중인 스레드의 스택**에 저장하고 `interrupt_handler` 를 호출한다.

interrupt_handler가 수행완료되면 레지스터 상태를 돌려놓고, `iretq` 를 호출한다.
- [assembly - what's the difference between iret and iretd,iretq? - Stack Overflow](https://stackoverflow.com/questions/11756153/whats-the-difference-between-iret-and-iretd-iretq)


**threads/loader.h**

```assembly
.section .text
.func intr_entry
intr_entry:
	/* Save caller's registers. */
	subq $16,%rsp
	movw %ds,8(%rsp)
	movw %es,0(%rsp)
	subq $120,%rsp
	movq %rax,112(%rsp)
	movq %rbx,104(%rsp)
	movq %rcx,96(%rsp)
	movq %rdx,88(%rsp)
	movq %rbp,80(%rsp)
	movq %rdi,72(%rsp)
	movq %rsi,64(%rsp)
	movq %r8,56(%rsp)
	movq %r9,48(%rsp)
	movq %r10,40(%rsp)
	movq %r11,32(%rsp)
	movq %r12,24(%rsp)
	movq %r13,16(%rsp)
	movq %r14,8(%rsp)
	movq %r15,0(%rsp)
	cld			/* String instructions go upward. */
	movq $SEL_KDSEG, %rax
	movw %ax, %ds
	movw %ax, %es
	movw %ax, %ss
	movw %ax, %fs
	movw %ax, %gs
	movq %rsp,%rdi
	call intr_handler
	movq 0(%rsp), %r15
	movq 8(%rsp), %r14
	movq 16(%rsp), %r13
	movq 24(%rsp), %r12
	movq 32(%rsp), %r11
	movq 40(%rsp), %r10
	movq 48(%rsp), %r9
	movq 56(%rsp), %r8
	movq 64(%rsp), %rsi
	movq 72(%rsp), %rdi
	movq 80(%rsp), %rbp
	movq 88(%rsp), %rdx
	movq 96(%rsp), %rcx
	movq 104(%rsp), %rbx
	movq 112(%rsp), %rax
	addq $120, %rsp
	movw 8(%rsp), %ds
	movw (%rsp), %es
	addq $32, %rsp
	iretq
.endfunc
```


## 인터럽트 발생 시 스택의 상태
![](/assets/images/Pasted%20image%2020230926170538.png)


## assembly: STI, CLI (x86)
- STI: Set Interrupt Flag
- CLI: Clear Interrupt Flag

https://modoocode.com/en/inst/sti
>The IF flag and the [STI](https://modoocode.com/en/inst/sti) and [CLI](https://modoocode.com/en/inst/cli) instructions do not prohibit the generation of exceptions and NMI interrupts. NMI interrupts (and SMIs) may be blocked for one macroinstruction following an [STI](https://modoocode.com/en/inst/sti).

### NMI
Non-Maskable Interrupt

주로 응답속도가 중요하거나, interrupt가 비활성화 되지 않아야 할때 사용된다.
- 복구가 불가능한 hardware 에러가 발생했을 경우
- system debugging, profiling 도중에
- 시스템 초기화 등 특별한 경우를 다루고 있을 때

최근에는 즉시 집중해야non-recoverable error

## 용어 정리
IRQ: Interrupt Request [Interrupt request - Wikipedia](https://en.wikipedia.org/wiki/Interrupt_request)
- 잠시 실행중인 프로그램을 멈추고 특수한 프로그램, Interrupt handler, 를 수행하도록 하는 요청
- 하드웨어 신호 (hardware signal)를 processor로 보내서 발생시킨다.
- 하드웨어 인터럽트는 주로 모뎀, 네트워크 인터페이스, 키보드 누름, 마우스 움직임 등에 의해 발생한다.
- 인터럽트 대응은 주로 IRQ의 number에 대응하는 index값으로 결정된다. -> IVT, IDT로 이어진다.
- 예를들어 Intel 8259 PIC는 IRQ0 ~ IRQ7에 대응하는 8개의 interrupt input이 있다.
	- 각 pin의 입력을 1bit라고 생각하면 자연스럽게 256가지의 경우가 나온다. (IVT의 크기)

IVT: Interrupt Vector Table [Interrupt vector table - Wikipedia](https://en.wikipedia.org/wiki/Interrupt_vector_table)
- 아키텍처에 관련없이 인터럽트의 핸들러의 리스트로 구성되어 있다.
- 각 Interrupt Number에 따른 Interrupt request
- 구현형태로는  Dispatch table, IDT가 있다.

![](/assets/images/Pasted%20image%2020230925222158.png)

IDT: Interrupt Descriptor Table [Interrupt descriptor table - Wikipedia](https://en.wikipedia.org/wiki/Interrupt_descriptor_table)
- [Intel 80286 - Wikipedia](https://en.wikipedia.org/wiki/Intel_80286) 및 이후로 사용되는 데이터 구조
- IVT를 구현한(implements) 내용이며, processor가 interrupt & exception에 대한 대응을 결정하도록 쓰인다.

DPL: Descriptor Privilege Level
- 인터럽트의 종류에 따라서 우선순위를 나타내는 값이다.
- PintOS는 모든 값을 0으로 설정한다.

