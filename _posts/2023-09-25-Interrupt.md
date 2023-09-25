---
title: Interrupt
aliases:
  - pintos-interrupt
tags: 
date created: 2023-09-25 21:30
---
dfdf

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
