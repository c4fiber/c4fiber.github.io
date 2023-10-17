---
title: How to Learn? and Problem Decomposition
aliases: 
tags:
  - learning
date created: 2023-10-17 20:49
---

## Fireship: How to Learn - 8 Hard Truths
---
https://www.youtube.com/watch?v=NtfbWkxJTHw

📚 Chapters
[00:00](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=0s) Learn to Code [00:43](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=43s) 
1. Hard Work [01:29](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=89s) 
2. Patterns over Syntax [02:38](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=158s) 
3. Stop Watching [03:22](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=202s) 
4. Stay Healthy [03:47](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=227s) 
5. Feynman Technique [05:00](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=300s) 
6. Dopamine Hits [05:47](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=347s) 
7. Not Too Hard tho [06:27](https://www.youtube.com/watch?v=NtfbWkxJTHw&t=387s) 
8. Learn Like a Pro

 간단하게 정리 해보자면
 1. 열심히 일하라
 2. 언어의 문법을 배우기(외우기)보다 pattern을 배워야 한다.
 3. 보기만 하지말고 직접 쳐봐라 (기타 등 악기 연습)
 4. 건강해라
 5. 파인만 테크닉 (velog에 1의 보수, 2의 보수 글)
 6. 작은 목표를 세우고 달성하면서 도파민을 자극하자
 7. 너무 열심히는 하지말자 (남들의 도움을 적극적으로 요청해라. stack overflow, twitter, discord 등)
 8. 구글링을 잘 활용하라?

problem decomposition 하고 연결되는 부분이 2번, 5번, 6번, 7번 내용으로 보인다.
- 프로그램을 설계하는 패턴을 잘 배워서 
- 파인만 테크닉을 통해 확실하게 이해한 뒤
- 문제를 잘게 쪼개고 (습득한 프로그래밍 패턴 + 이해한 지식 활용)
- 팀원들에게 적극적으로 물어보고 도움을 청한다.

## Problem Decomposition

https://youtu.be/bmSAYlu0NcY?si=_Q421m2LXEg-v1_i

pintOS 강의 첫번째 시간에 권영진 카이스트 교수님께서 추천해주신 내용이다.

computer science에서 가장 중요한 concept를 하나 뽑자면
- Abstraction
- Complexity
- Layer of indirection 
등이 있겠지만 John Ousterhout 가 생각하는 건 **Problem Decomposition** 이라고 한다.

이와 더불어 UNIX 파일 시스템 이야기나 (아름답다고 극찬한다. 특히 file deletion의 문제를 해결하는 과정에서), TCL 스크립트를 개발하고 Software Design에 관한 boot camp 비슷하게 강의를 하는데 (Unset 이라는 명령어에서 발생만 문제) 꽤 흥미로웠다.
Exception에 대한 이야기도 하는데, 본인은 이전에 문제가 생길만한 부분을 전부 모아서 던져버린다 (Handler에게 모두 맡겨버린다, 내지는 더이상 신경쓰지 않는다) 라고 표현했다. 하지만 이는 잘못됐다고 하는데 만약 설계에서 문제가 생긴다면 어떻게 해결할지, 두 문제가 충돌한다면 win-win 하게 해결할 수 없는지 (unix file deletion) 를 고민해야 한다고 이야기 했다.

