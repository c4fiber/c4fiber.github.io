---
title: Rio_readinitb의 역할
aliases: 
tags: 
date created: 2023-09-20 07:14
---
## 발단

프록시 서버를 구축하는 과정에서 에러가 발생했다.
간헐적으로 데이터를 제대로 전송하지 못해서 발생하는 에러였다.

특히 html 파일을 전달받을 때 데이터 용량은 동일했지만 html file이 아닌 binary 파일로 인식하는게 가장 큰 문제 였다.

## 전개 

이를 해결하기 위해서 `<!DOCTYPE html>` 을 추가했다. -> 실패

`<meta charset="utf-8"` 도 추가해 보았다. -> 실패

## 위기, 절정 ,결말

`Rio_readinitb(&rio, target_fd)` 코드를 통해 target_fd 소켓 디스크립터와 &rio를 연결한다.

![](/assets/images/Pasted%20image%2020230920174837.png)
이때 rio_bufptr을 rio_buf의 초기지점으로 정하는 걸 알 수 있다.
사실상 버퍼를 초기화 하는 과정이라고도 볼수 있는데


![](/assets/images/Pasted%20image%2020230920071644.png)

다음과 같이 내 코드에서는 target으로 부터 받은 response header를 받고 contents를 받을 때 또 다시 Rio_readinitb()를 호출했다.

만약 이 시점에서 header의 data를 완전히 buffer에 읽어들이지 못하고 rio_buffer에만 남아 있다면
나는 그 데이터를 잃어버리게 되는 것이다.

이 때문에 간헐적으로 데이터 전송이 완전히 이루어지지 못해서 프록시 서버가 제대로 작동하지 못했다.

