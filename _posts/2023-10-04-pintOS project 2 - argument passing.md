---
title: 
aliases: 
tags: 
date created: 2023-10-04 21:03
---
# 시작
process.c 파일의 process_exec 를 보면 elf 파일을 load 하는 과정이 있다.

load 함수는 process.c 파일에 정의되어 있으며 최 하단에 보면 TODO 로 argument passing을 구현하라고 주석처리 되어있음

현재 file_name 은 인자를 포함한 형태 “test a b c” 이며 이를 parsing 해서

실행 파일은 test, 인자로 a, b, c 를 넘겨주도록 해야함.


# 과정

## input arguments into stack

저장시점: `load` 함수 내 `set_stack` 을 수행하고 나면 `struct intr_frame if_` 의 `rsp` 값에 stack top 주소가 들어온다.

이 rsp값을 낮춰가면서 stack에 데이터를 집어 넣는다.

![](/assets/images/Pasted%20image%2020231004211055.png)



![](/assets/images/Pasted%20image%2020231004194633.png)

![](/assets/images/Pasted%20image%2020231004194647.png)

다음 형식에 맞춰서 rsp에 데이터를 저장한 모습이다.

현재 스택을 모두 넣은 상태에서 rsp 는 `0x4747ffc8` 이다.
gdb로 출력해본 결과 값이 잘 들어간걸 볼 수 있다.

차이점이라면 introduction 에서는 word-align이 추가되고 그 다음부터 argv의 실제 내용이 저장된다.
하지만 나는 NULL로 채워진 `argv[argc]` 다음에 바로 저장되게 하고, 마지막에 padding을 붙여서 align을 맞췄다.


# ERROR
## process_exec, load 수정 후 "system call!" 에러

![](/assets/images/Pasted%20image%2020231004210520.png)

부팅이 완료되고 args-single 을 실행하는 과정에서 에러가 발생한다.

`system call!` 이라는 출력과 함께 args-single onearg가 종료되었다고 나온다.

![](/assets/images/Pasted%20image%2020231004210653.png)

syscall_handler가 호출되었을 때 상태가 no_sti 인데
이 no_sti 의 내용을 확인할 수 없어서 멈춰있는 중이다.


**thread.c**
![](assets/images/Pasted%20image%2020231004211548.png)

다음와 같이 yield 하는 조건을 수정했더니 문제가 없어졌다. -> 문제 해결 방법이 아니다.
이 경우 main thread가 새로운 thread를 만들고 실행시켜야 하는데 yield 하지 않아서 main thread가 바로 종료되어 버린다. 
이 때문에 새로 생성된 스레드는 실행되지 못하고 내가 삽입한 실행파일 `args-single` 이 실행되지 못한다.


