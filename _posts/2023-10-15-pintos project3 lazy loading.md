---
title: 
aliases: 
tags: 
date created: 2023-10-15 14:51
---
## 흐름 정리
---
![](/assets/images/Pasted%20image%2020231015145214.png)

uninit_page 는 동그라미
anon_page(세모), file_page(네모)로 만들어줘야 한다.
- 이를 위해서 vm_alloc_with_initializer 함수에서 switch문을 통해 선택한다.

해당 페이지를 사용하기 위한 사전작업을 정의한다
- vm_alloc_with_initializer가 전달받은 init 함수를 등록한다.(vm_initializer 형태를 갖춘다.)
- init함수를 실행하기 위해 필요한 인자를 aux를 통해 전달받는다.






## 문제 발생 지점
---


## 고려해야 할 부분
---