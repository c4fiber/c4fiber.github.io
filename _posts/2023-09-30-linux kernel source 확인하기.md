---
title: 
aliases: 
tags: 
date created: 2023-09-30 01:50
---
pthread 에서 prio_inherit 옵션이 있는 걸 보고 어떻게 구현되어 있는지 알고싶어졌다.

reference 등을 통해서는 어떤 헤더파일에 정의되어 있는지, 반환값이 어떤지 간단하게 설명이 되어있다.

하지만 원본 소스를 확인할 수 없어서 소스코드를 확인해보려고 찾아본 결과 다음과 같은 사이트를 찾을 수 있었다.

https://git.kernel.org/pub/scm/linux/kernel/git/stable/linux.git/tree/kernel/locking/mutex.c?h=v6.5.5

몇가지 알아낸 바로는
1. 비트마스킹을 이용해서 하위 1비트로 RT_MUTEX_HAS_WAITERS 를 바로 체크할 수 있다.
2. 