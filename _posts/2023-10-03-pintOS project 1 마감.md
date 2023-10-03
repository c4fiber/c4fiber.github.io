---
title: pintOS project 1 마감pintos-project 1
aliases:
  - pintos-project2-end
tags: 
date created: 2023-10-03 15:42
---

# week07 PintOS project 1-2

pintos-kaist git: https://casys-kaist.github.io/pintos-kaist/

작업 repository: https://github.com/c4fiber/pintos-kaist/branches

- pintos 루트 디렉토리에 환경세팅하기 -> .bashrc에 저장해두면 매번 수행할 필요가 없어진다.
![](/assets/images/Pasted%20image%2020230921172044.png)
- `pintos -- run alarm-multiple > logfile` serial 출력을 log로 저장하는 실행 방법
	- 옵션을 추가한다면 -- 앞에다가 추가해라 -> --를 기준으로 앞은 옵션, 뒤는 인자로 나뉜다.
- test를 진행할 때 `VERBOSE=1` 옵션을 추가하면 테스트 진행상황을 볼 수 있다.


## 1. Alarm clock
[2023-09-24-pintos Alarm clock](_posts/2023-09-24-pintos%20Alarm%20clock.md)


## Threads
[2023-09-22-Threads](_posts/2023-09-22-Threads.md)

## semaphore, lock, monitor
semaphore, lock, monitor: [2023-09-21-Synchronization](_posts/2023-09-21-Synchronization.md)

lock(mutex) 와 세마포어 이해하기: [2023-09-25-mutex vs semaphore](_posts/2023-09-25-mutex%20vs%20semaphore.md)


## interrupt
[pintos-interrupt](_posts/2023-09-25-Interrupt.md)

iret, iretw, iretq가 무엇인가? : [assembly - what's the difference between iret and iretd,iretq? - Stack Overflow](https://stackoverflow.com/questions/11756153/whats-the-difference-between-iret-and-iretd-iretq)

## 2. priority inversion + priority donation
[2023-09-30-linux kernel source 확인하기](_posts/2023-09-30-linux%20kernel%20source%20확인하기.md)

[2023-09-30-priority inversion](_posts/2023-09-30-priority%20inversion.md)

## Error handling

[2023-09-24-pintos ERROR](_posts/2023-09-24-pintos%20ERROR.md)
