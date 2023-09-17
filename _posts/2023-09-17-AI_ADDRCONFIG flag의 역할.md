---
title: AI_ADDRCONFIG flag의 역할
aliases: 
tags: 
date created: 2023-09-17 14:39
---
## 왜 hints.ai_family = AF_INET을 수행하지 않는가?

![](/assets/images/Pasted%20image%2020230917144308.png)

p. 970 csapp.c 내용을 보면 hints.ai_family = AF_INET을 설정하는 부분이 없다.
해당 코드는 IPv4를 사용하겠다는 hint를 제공하기 위함인데 왜 사용하지 않았을까?

답은 `hints.ai_flags |= AI_ADDRCONFIG` 에 있다.
해당 flag를 set하게 되면 IPv4 주소에 대한 query는 반드시 수행한다.
추가로 request하는 노드의 IPv6 주소가 세팅되었는지 확인하며 적어도 하나의 IPv6주소를 가지고 있다면 IPv6에 대해서도 query를 수행한다.

IPv4를 기본으로 깔고 IPv6 주소도 사용할 준비가 되어있다면 추가로 query를 해 연결확률을 조금 더 높여준다고 이해했다.

**참고자료**
- https://www.ibm.com/docs/en/zvse/6.2?topic=SSB27H_6.2.0/fa2ti_call_getaddrinfo.htm
- https://man7.org/linux/man-pages/man3/getaddrinfo.3.html



