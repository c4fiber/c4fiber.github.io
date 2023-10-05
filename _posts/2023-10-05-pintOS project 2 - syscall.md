---
title: pintOS project 2 - syscall
aliases:
  - pintos-project2-syscall
tags: 
date created: 2023-10-05 23:31
---

# 현재까지 진행상황

`lib/user/syscall.h` 를 기반으로 기초적인 형태만 구축하면 다음과 같이 에러가 발생한다.

![](assets/images/Pasted%20image%2020231005233254.png)

switch 문을 사용해서 system call number에 대해 처리를 수행한다.
해당 내용은 `include/lib/syscall-nr.h` 에 enum으로 선언되어 있다.
ex) SYS_HALT 는 0, SYS_WRITE 10 로 되어있다.

![](assets/images/Pasted%20image%2020231005233358.png)

