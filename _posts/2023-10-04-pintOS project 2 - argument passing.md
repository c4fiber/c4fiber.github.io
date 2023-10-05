---
title: pintos-project2-argument passing
aliases:
  - pintos-project2
tags: 
date created: 2023-10-04 21:03
---
# 시작

## 왜? (목적)

현재 file_name 은 인자를 포함한 형태 “test a b c” 이다.
filesys 를 통해서 file을 읽어들여야 하는데 공백을 기준으로 파일명 (예시에서는 "test")를 분리해야 한다.

이를 parsing 해서 실행 파일은 test, 인자로 a, b, c 를 넘겨주도록 해야함.

## 어떻게? (방법)

process.c 파일의 process_exec 를 보면 elf 파일을 load 하는 과정이 있다.

load 함수는 process.c 파일에 정의되어 있으며 최 하단에 보면 TODO 로 argument passing을 구현하라고 주석처리 되어있음

# 과정

## input arguments into stack

저장시점: `load` 함수 내 `set_stack` 을 수행하고 나면 `struct intr_frame if_` 의 `rsp` 값에 stack top 주소가 들어온다.

이 rsp값을 낮춰가면서 stack에 데이터를 집어 넣는다.

![](/assets/images/Pasted%20image%2020231004211055.png)
π


![](/assets/images/Pasted%20image%2020231004194633.png)

![](/assets/images/Pasted%20image%2020231004194647.png)

다음 형식에 맞춰서 rsp에 데이터를 저장한 모습이다.

현재 스택을 모두 넣은 상태에서 rsp 는 `0x4747ffc8` 이다.
gdb로 출력해본 결과 값이 잘 들어간걸 볼 수 있다.

차이점이라면 introduction 에서는 word-align이 추가되고 그 다음부터 argv의 실제 내용이 저장된다.
하지만 나는 NULL로 채워진 `argv[argc]` 다음에 바로 저장되게 하고, 마지막에 padding을 붙여서 align을 맞췄다.

## 번외: hex_dump 를 이용해 확인하는 방법

**ㅔ
![](assets/images/Pasted%20image%2020231005165420.png)

![](assets/images/Pasted%20image%2020231005165357.png)


# ERROR
## process_exec, load 수정 후 "system call!" 에러

![](/assets/images/Pasted%20image%2020231004210520.png)

부팅이 완료되고 args-single 을 실행하는 과정에서 에러가 발생한다.

`system call!` 이라는 출력과 함께 args-single onearg가 종료되었다고 나온다.

![](/assets/images/Pasted%20image%2020231004210653.png)

syscall_handler가 호출되었을 때 상태가 no_sti 인데
이 no_sti 의 내용을 확인할 수 없어서 멈춰있는 중이다.


**thread.c**
![](/assets/images/Pasted%20image%2020231004211548.png)

다음와 같이 yield 하는 조건을 수정했더니 문제가 없어졌다. -> 문제 해결 방법이 아니다.
이 경우 main thread가 새로운 thread를 만들고 실행시켜야 하는데 yield 하지 않아서 main thread가 바로 종료되어 버린다. 
이 때문에 새로 생성된 스레드는 실행되지 못하고 내가 삽입한 실행파일 `args-single` 이 실행되지 못한다.

### (임시) solved: thread_create 에서 yield 방법 변경 

확인된 원인은 thread_yield 에서 발생하는 assertion failed 이다.
`ASSERT (!intr_context())`  코드에서 실패하고 종료된다.

`synch.c` 코드에서 `sema_up()` 을 확인해보면 yield() 만 사용했다.
정확한 지점은 추적이 불가능하지만 external_interrupt 상황에서 yield를 수행할 경우 발생한다.

이를 막아주기 위해서 외부 인터럽트 상황일 때 `intr_yield_on_return()` 으로 해결해주었다.

![](/assets/images/Pasted%20image%2020231005053002.png)




## intr 0x0d General Protection exception

![](/assets/images/Pasted%20image%2020231005014920.png)

원인은 찾지 못했다. call stack으로 추적할 수가 없었다.

### 해결시도 1: 8byte 단위로 뒤집어주기 -> 실패

이때 레지스터 상태를 보면 `6E69732D 73677261` 이라는 값이 들어있다.

![](/assets/images/Pasted%20image%2020231005052647.png)

hex값으로 보면 "args-sin" 이 뒤집어있는걸 볼 수 있는데 이때문에 string을 8byte 단위로 뒤집어서 넣어야 하나? 라는 생각을 잠깐했다.

너무 갔다고 생각했다.


# 문제 해결 방법

## `%rsi` 에는 argv\[0]의 **주소 값** 이 들어가야 한다. 즉 `rsp - 8` 의 값이 들어가야 한다.

![](/assets/images/Pasted%20image%2020231005052252.png)
![](/assets/images/Pasted%20image%2020231005052303.png)


나의 코드 (process.c)
![](/assets/images/Pasted%20image%2020231005052237.png)

3시간은 쓴거같다.

## exit(status_code) 를 수행하면 이를 출력해주어야 한다.

![](/assets/images/Pasted%20image%2020231005052402.png)


## 스레드를 생성할 때 이름을 제대로 집어넣지 못했다.

꼭 한줄씩 문제가 생겼는데 `test a b c`  라는 입력이 들어왔으면 스레드의 이름을 `test` 로 해줘야 하기 때문이였다.

다음과 같이 스레드 생성에 필요한 이름을 파싱하기 위해 `sub_file_name` 을 만들었다.

`strcspn` 함수는 reject가 포함되지 않는 index 0 부터 시작하는 substring의 길이를 반환한다.
char 배열의 크기가 15인 이유는 pintOS 에서 파일이름의 길이가 최대 14였기 때문이다.

![](/assets/images/Pasted%20image%2020231005051833.png)

결과:

![](/assets/images/Pasted%20image%2020231005051757.png)