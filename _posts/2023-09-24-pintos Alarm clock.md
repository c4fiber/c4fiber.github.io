---
title: pintos project 1
aliases:
  - pintos project 01
tags: 
date created: 2023-09-24 23:08
---
busy wait 방식으로 thread_sleep으로 구현된 코드를 바꾸기 위해서 작성한 로직

blocked_list 를 생성해서 sleep 상태인 스레드는 block(status = blocked, blocked_list에 추가) 처리한다.

매 timer_interrupt가 발생했을 때 blocked_list를 탐색하면서 깨어나야할 스레드를 확인하고 unblock (ready_queue에 삽입 & status = ready)을 수행한다.

![](/assets/images/Pasted%20image%2020230924230953.png)



![](/assets/images/Pasted%20image%2020231002191950.png)
