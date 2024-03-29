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


## no "Timer: # ticks" message
---
![](assets/images/Pasted%20image%2020231018133517.png)

init.c 의 main 함수 부분을 보면 run task라는 곳을 볼 수 있다.
이 부분에서 USERPROG의 경우 유저 프로그램을 실행시키도록 스레드를 생성하는데

우리가 진행한 테스트는 fork-once 였다.

실행파일을 로딩하는 과정에서 0x3ff000 부분을 할당해서 로딩해줘야 하기 때문에 이를 수행해 줬는데

fork-once의 경우 부모 프로그램이 실행되면서 이부분에 fault를 발생시키지 않는다.
이 때문에 aux를 여전히 보유한채로 supplement page copy가 수행되고, 자식 프로그램의 page는 aux가 담긴 주소값을 그대로 가져온다.

문제는 child가 process exit을 호출할때 발생하는데
이 과정에서 (process_cleanup에서 ) spt page kill을 수행하고 보유한 정보를 모두 free 시키는데 여기에 부모에게서 받은 aux가 존재한다.

child가 종료된 이후 부모도 exit을 호출할때 spt page kill을 수행할 때 보유한 aux에 대해 free를 한번 더 수행하게 된다.

이때문에 프로그램이 강제로 종료되고, fork-once complete라는 결과는 출력되지만
그 이후에 나오는 timer ticks부분이 제대로 출력되지 않기 때문에 테스트는 통과하지 못했다.