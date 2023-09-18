---
title: 
aliases: 
tags: 
date created: 2023-09-19 02:10
---

[SIGPIPE](_posts/2023-09-19-SIGPIPE%20handle.md) 문제를 잘못인식하고 데이터를 잘라서 보내야 하는건가? 라는 의심에 시도해본 방법중 하나이다.

## HTTP status code 206

대용량 파일 전송이나, 비디오 스트리밍 등에 주로 사용된다.

큰 용량의 데이터를 한 번에 받기 힘든 사용자는 data의 일부분을 요구할 수 있다.
이 요청이 처리될 경우 server는 HTTP 206 response를 보낸다.

## 요청이 처리되는 순서

1. 서버 측에서 전달받은 HTTP header의  ` Accept-Ranges:` 를 통해 부분전송 가능여부를 확인한다.
`none` 일경우 불가능, `bytes` 의 경우 byte 단위로 끊어서 전송할 수 있다는 뜻이다.

2. 클라이언트는 헤더에 `Range: start-end` 를 추가한다. start ~ end까지의 bytes를 전달해달라는 뜻이다.

3. 서버는 데이터를 전송할 때 `Content-Range: start-end/total size` 를 헤더에 추가한다.
    동시에 `HTTP 206` Response를 전달한다.




## 참고자료
https://developer.mozilla.org/ko/docs/Web/HTTP/Status/206