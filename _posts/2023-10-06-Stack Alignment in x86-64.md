---
title: Stack Alignment in x86-64
aliases: 
tags:
  - x86-64
  - alignment
  - pintos
date created: 2023-10-06 22:23
---
# 왜 공부하게 되었나?

[pintos-project2-syscall](_posts/2023-10-05-pintOS%20project%202%20-%20syscall.md) 을 공부하다가 우연히 


x86-64 에는 calling convention이 존재한다.
https://en.wikipedia.org/wiki/X86_calling_conventions#System_V_AMD64_ABI

이에 따르면 함수 호출(call) 직전의 stack의 상태에서 rsp의 값이 16의 배수여야 함을 알리고 있다.

alignment는 performance를 위한 권장사항이라고 한다.




하지만 특정 명령어 (MOVAPS)는 이를 강제하기도 한다.

# 어떻게 공부했는가? HOW?

Intel에서 제공하는 IA-32 아키텍처 개발자 매뉴얼 pdf를 읽었다.
블로그 등 게시물 내용을 못믿는것도 있고 내가 필요한 지식이 항상 담겨져 있지 않았다.



## Intel® 64 and IA-32 Architectures Software Developer Manuals

(Vol.1 4-1) 프로그램과 자료 구조(특히 stack)의 성능을 위해서 alignment를 수행해야 한다고 나와있다.

![](/assets/images/Pasted%20image%2020231006222640.png)

---

(Vol. 3A 6-20) XMM 레지스터를 사용하는 MOVAPS 명령어의 경우 alignment를 요구한다
![](/assets/images/Pasted%20image%2020231006223431.png)

(Vol.1 5-17) MOVAPS 명령어는 "정렬된" 4개의 packed floating-point values를 이동시킨다고 나온다.
![](/assets/images/Pasted%20image%2020231006223512.png)

# 참고자료
Intel® 64 and IA-32 Architectures Software Developer Manuals: https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html

