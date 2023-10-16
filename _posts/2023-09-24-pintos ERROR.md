---
title: 
aliases: 
tags: 
date created: 2023-09-24 16:55
---

## undefined refernce to 'idle'
---

![](/assets/images/Pasted%20image%2020230924165604.png)

![](/assets/images/Pasted%20image%2020230924165655.png)

문자열을 그냥 입력했는데 왜 reference를 찾지 못한다고 하는 걸까...

-> 그 이전에 작성했던 코드가 문제였다 현재는 해결 완료.

## page fault exception
---

![](/assets/images/Pasted%20image%2020231001035001.png)

priority-donate-one 완성 후 multiple로 코드를 수정하는 과정에서 발생했다.
수정한 코드는 synch.c의 lock_acquire, lock_release 함수이다.

## priority-donate_lower
---
테스트 수행
![](/assets/images/Pasted%20image%2020231001163529.png)

결과
![](/assets/images/Pasted%20image%2020231001163729.png)

main thread의 priority를 임의로 낮춰버리고 나의 priority를 확인해도 donation을 받은 priority가 존재해야 한다는 조건이 있다.

내가 낮추고 싶어도 donation이 존재하는 한 그보다 낮출 수 없는 조건을 확인하는 코드이다.

이를 위해서는 thread_set_priority 에서 내 donor_list를 확인해서 받은 priority의 최대값이 설정할 priority보다 낮을 경우 무시하도록 설정해야 한다.


## list_next가 실패해서 kernel panic이 발생하는 경우
---
![](assets/images/Pasted%20image%2020231016115323.png)

args 시리즈에서 나머지는 다 되는데 args-many만 수행되지 않는다.
hash_find를 통해서 페이지가 존재하는지 확인하는 작업인데, 이상하게 여기서만 터진다.

이전에도 list_sort를 수행하면서 list_next에서 터진적이 있었는데 비슷한 케이스로 보인다.