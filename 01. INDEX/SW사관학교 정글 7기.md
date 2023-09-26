---
title: SW사관학교 정글 7기
aliases:
  - 정글
  - SW사관학교
  - 정글7기
tags: 
date created: 2023-09-16 16:27
date modified: 2023-09-17 14:51
---


# week 05 malloc lab


# week 06 웹서버 만들기

C 언어로 작성된 소켓 프로그래밍을 기반으로 한다.


### 11.4 The Sockets Interface

[2023-09-16-서버와 클라이언트가 연결하는 방법](_posts/2023-09-16-서버와%20클라이언트가%20연결하는%20방법.md)


open_clientfd()
[2023-09-17-AI_ADDRCONFIG flag의 역할](_posts/2023-09-17-AI_ADDRCONFIG%20flag의%20역할.md)


### 참고한 정답 코드

proxy: cached 
https://github.com/vmsandeeprao/HTTP-proxy-server/blob/master/proxy.c


# week07 PintOS project 1-2

pintos-kaist git: https://casys-kaist.github.io/pintos-kaist/


- pintos 루트 디렉토리에 환경세팅하기 -> .bashrc에 저장해두면 매번 수행할 필요가 없어진다.
![](assets/images/Pasted%20image%2020230921172044.png)
- `pintos -- run alarm-multiple > logfile` serial 출력을 log로 저장하는 실행 방법
	- 옵션을 추가한다면 -- 앞에다가 추가해라 -> --를 기준으로 앞은 옵션, 뒤는 인자로 나뉜다.
- test를 진행할 때 `VERBOSE=1` 옵션을 추가하면 테스트 진행상황을 볼 수 있다.


semaphore, lock, monitor: [2023-09-21-Synchronization](_posts/2023-09-21-Synchronization.md)

lock(mutex) 와 세마포어 이해하기: [2023-09-25-mutex vs semaphore](_posts/2023-09-25-mutex%20vs%20semaphore.md)

권영진 교수님 pintOS강의: [abstraction](_posts/2023-09-26-PintOS%20강의.md) 

iret, iretw, iretq가 무엇인가? : [assembly - what's the difference between iret and iretd,iretq? - Stack Overflow](https://stackoverflow.com/questions/11756153/whats-the-difference-between-iret-and-iretd-iretq)


참고도서: https://pages.cs.wisc.edu/~remzi/OSTEP/

번역본: https://github.com/remzi-arpacidusseau/ostep-translations/tree/master/korean

