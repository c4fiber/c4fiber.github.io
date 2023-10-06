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



# system call 호출 과정

init.c -> main 에서 syscall_init() 을 통해 system_call을 처리할 수 있도록 한다.

![](assets/images/Pasted%20image%2020231006192117.png)

이전에는 system call을 interrupt를 통해 처리했지만 (0x80, linux 기준) x86-64 아키텍처에서는 명령어로 처리할 수 있게 지원해준다.

이때 사용하는 MSR(Model Specific register) 레지스터가 존재한다.

---

![](assets/images/Pasted%20image%2020231006192308.png)

btsq 어셈블리 명령어로 r11 레지스터의 9번째 bit를 세팅한다. 이때 기존 bit가 0인지 확인한다.
0이 였다면 `CARRY FLAG` 를 1로 세팅한다.
jnb 명령어로 carry FLAG를 확인하고 으로 세팅되어 있으면 no_sti로 점프한다.

즉 인터럽트가 활성화 상태라면  `no_sti`를 수행하고, 아니라면 sti를 수행하는 코드이다.

---

**no_sti** (syscall-entry.S)

![](assets/images/Pasted%20image%2020231006210332.png)

지금까지 syscall_handler를 호출한 주체를 찾았는데 여기에 있었다.
syscall_handler를 호출하고 stack에 백업해둔 general register들을 복원하는 작업이다.



이를통해
1. syscall 호출시에 인터럽트는 활성화 되어있어야 한다.
2. pintos 는 r11을 eflags register로 활용한다.





r11 레지스터는 `EFLAGS register` 로 사용되는데 이때 9번째 bit는 `IF FLAG` 이다.
즉 인터럽트 비활성화 상태인지 확인하고 그렇다면 (0 이였다면 현재 인터럽트 비활성화 상태임을 의미한다.)




# 참고자료

assembly constrains
https://gcc.gnu.org/onlinedocs/gcc/Machine-Constraints.html

