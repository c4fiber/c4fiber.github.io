---
title: SIGPIPE handle
aliases:
  - socket
  - c socket
  - SIGPIPE
tags: 
date created: 2023-09-19 02:00
---
## SIGPIPE error는 왜 발생하는가?

소켓 통신을 통해서 client에게 데이터를 전달하던 도중 client가 소켓을 닫아버리는 경우가 있다.

정확한 원인은 알수없지만 추측하기로는
- 요청한 데이터가 이미 캐싱되어 보유하고 있음을 판단했을 때
- 클라이언트가 다른 페이지로 이동해서 더이상 데이터가 필요없을 때

이때 클라이언트는 소켓을 닫아버려도 서버는 데이터를 connfd를 통해 데이터를 계속 보내려고 한다.
이 순간 서버는 SIGPIPE(Connection reset by peer, broken pipe)에 직면하게 된다.


## 어떻게 해결할 것인가?

해결 방법은 두가지가 있다.
1. SIGPIPE 에러 자체를 무시해버린다.
	- `signal(SIGPIPE, SIG_IGN)` 를 호출한다. (주로 main에서 이루어짐)
2. Socket을 생성할 때 SIGPIPE 에러를 무시하도록 설정한다.
	- `setsockopt(sd, SOL_SOCKET, SO_NOSIGPIPE, (void *)&set, sizeof(int))` 

2번의 방법은 send()를 사용할 때 적용된다고 한다 (범용적이지 않아서 매번 적용되는지 알 수 없다, linux에서는 없을 수도 있음)
따라서 2번의 경우는 `MSG_NOSIGNAL` 을 사용하는 방법으로 선회해야한다고 한다.


나는 csapp.c 파일을 수정하지 말아야 한다는 원칙아래 1번의 방법을 선택했다.

시간 진짜 많이 썼다...



## 참고 자료
https://stackoverflow.com/questions/108183/how-to-prevent-sigpipes-or-handle-them-properly

https://stackoverflow.com/questions/108183/how-to-prevent-sigpipes-or-handle-them-properly




