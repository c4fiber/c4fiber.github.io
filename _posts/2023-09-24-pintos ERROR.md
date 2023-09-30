---
title: 
aliases: 
tags: 
date created: 2023-09-24 16:55
---

## undefined refernce to 'idle'

![](/assets/images/Pasted%20image%2020230924165604.png)

![](/assets/images/Pasted%20image%2020230924165655.png)

문자열을 그냥 입력했는데 왜 reference를 찾지 못한다고 하는 걸까...

-> 그 이전에 작성했던 코드가 문제였다 현재는 해결 완료.

## page fault exception

![](/assets/images/Pasted%20image%2020231001035001.png)

priority-donate-one 완성 후 multiple로 코드를 수정하는 과정에서 발생했다.
수정한 코드는 synch.c의 lock_acquire, lock_release 함수이다.

