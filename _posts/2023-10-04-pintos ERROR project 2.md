---
title: 
aliases: 
tags: 
date created: 2023-10-04 19:45
---

# argument passing

## input arguments into stack

![](assets/images/Pasted%20image%2020231004194633.png)

![](assets/images/Pasted%20image%2020231004194647.png)

다음 형식에 맞춰서 rsp에 데이터를 저장한 모습이다.

현재 스택을 모두 넣은 상태에서 rsp 는 `0x4747ffc8` 이다.
gdb로 출력해본 결과 값이 잘 들어간걸 볼 수 있다.
차이점이라면 introduction 에서는 word-align이 추가되고 그 다음부터 argv의 실제 내용이 저장된다.
하지만 나는 NULL로 채워진 `argv[argc]` 다음에 바로 저장되게 하고, 마지막에 padding을 붙여서 align을 맞췄다.ㅊ