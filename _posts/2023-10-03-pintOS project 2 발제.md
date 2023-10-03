---
title: pintOS project 2 발제
aliases:
  - pintos-project2-start
tags: 
date created: 2023-10-03 13:31
---
## TDD test driven development

spec (specification)을 test로 명세한다.

안다고 착각하기 쉽다 -> 구체적으로 한다. (지식을 구체화 시키면서 자기 검증?)

**Timer** 없이는 스레드 구현이 안된다. -> hardware의 도움을 받아서 스레드를 구현한다.

왜 context를 저장하는가? -> 다른 프로그램이 레지스터를 쓰니까! -> 원래의 내용과 달라질 수 있으니까!


## condvar
condvar -> monitor를 만든다 (2개, 3개, 등등 여러개의 스레드가 들어올 수 있게 할 수 있다.)
lock을 왜 거는가? -> 무결성(integrity) 도 있지만 조금더 low 하게 가면 Atomicity (원자성)이다. -> critical section을 수행하기 위해서 이다.


## mlfq를 사용했을 때 priority scheduling과의 비교

priority scheduling을 사용했을 때 priority inversion이 발생할 수 있다. -> 그 둘만 deadlock이 걸린다. -> running 상태의 스레드가 없다.


## project 2에서 할 일들
call, ret (rdi)의 연관성, call, jmp 차이를 정확히 알고가야 한다.
binary file을 분석한다.

process는 computer를 추상화 한 것이다.

user - kernel , user - user 사이에 protection을 구현해야 한다.

argument passing -> specification 은 명세되어 있다.
