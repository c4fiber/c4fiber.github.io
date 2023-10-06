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
---
[pintos-project2-syscall](_posts/2023-10-05-pintOS%20project%202%20-%20syscall.md) 을 공부하다가 우연히 FAQ에서 stack alignment에 대한 내용을 봤다.

# 16byte Alignment
---
x86-64 에는 calling convention이 존재한다.
https://en.wikipedia.org/wiki/X86_calling_conventions#System_V_AMD64_ABI

이에 따르면 함수 호출(call) 직전의 stack의 상태에서 rsp의 값이 16의 배수여야 함을 알리고 있다.

alignment는 performance를 위한 권장사항이라고 한다.

## Intel® 64 and IA-32 Architectures Software Developer Manuals

(Vol.1 4-1) 프로그램과 자료 구조(특히 stack)의 성능을 위해서 alignment를 수행해야 한다고 나와있다.

![](/assets/images/Pasted%20image%2020231006222640.png)

---

(Vol. 3A 6-20) IA-32e mode에서는 stack frame을 생성하기 전 RSP의 16byte alignment를 요구한다
![](/assets/images/Pasted%20image%2020231006223431.png)

---

(Vol.1 5-17)  몇몇개의 명령어는 16byte alignment를 강제한다.
그중 하나는 `MOVAPS` 명령어 이다. 부동소수점 값을 복사하기 위한 명령어인데 
XMM 레지스터를 사용하고, **정렬된** 4개의 packed floating-point values를 이동시킨다고 나온다.
![](/assets/images/Pasted%20image%2020231006223512.png)

# 운 좋게 들어맞았던 argument passing
---
[2023-10-04-pintOS project 2 - argument passing](_posts/2023-10-04-pintOS%20project%202%20-%20argument%20passing.md) 을 수행할 때는 얻어걸렸었다.

![](/assets/images/Pasted%20image%2020231004211055.png)

다음과 같이 RSP+8 의 값이 0x4747FFD0 로 16byte 정렬이 되어있었다.

---

물론 pintOS에서는 XMM 레지스터를 사용하지 않고, 부동소수점 연산을 지원하지 않는다.

![](/assets/images/Pasted%20image%2020231006230043.png)

MOVAPS 같은 부동소숫점 연산 명령어를 사용할 일은 없고, 16byte alignment 가 아닌 8byte alignment만 수행해도 프로그램에 문제는 없다.

# 참고자료
---
Intel® 64 and IA-32 Architectures Software Developer Manuals: https://www.intel.com/content/www/us/en/developer/articles/technical/intel-sdm.html

pintos-kaist document: https://casys-kaist.github.io/pintos-kaist/project2/introduction.html
