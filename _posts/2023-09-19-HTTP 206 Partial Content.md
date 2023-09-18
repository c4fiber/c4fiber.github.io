---
title: 
aliases: 
tags: 
date created: 2023-09-19 02:10
---
## HTTP status code 206

대용량 파일 전송이나, 비디오 스트리밍 등에 주로 사용된다.

큰 용량의 데이터를 한 번에 받기 힘든 사용자는 data를 요구하면서 헤더에 Range를 추가할 수 있다.
Range에 해당하는 데이터를 잘라서 보내달라는 뜻인데
Range: \<start\>-\<end\> 형태로 주어지며 요청하는데이터의 범위를 나타낸다.

이러한 작업은 서버 측에서 ` Accept-Ranges:` 를 통해 부분전송 가능여부를 알려준다.
`none` 일경우 불가능, `bytes` 의 경우 byte 단위로 끊어서 전송할 수 있다는 뜻이다.

서버는 이에 따라서 데이터의 일부분을 전송할 수 있으며 전송할 때 


## 참고자료
https://developer.mozilla.org/ko/docs/Web/HTTP/Status/206