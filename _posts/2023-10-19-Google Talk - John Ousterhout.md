---
title: Google Talk - John Ousterhout
aliases: 
tags:
  - Abstraction
  - SoftwareDesign
date created: 2023-10-19 02:48
---
## A Philosophy of Software Design | John Ousterhout | Talks at Google
---
pintOS 강의를 들으면서 권영진 카이스트 교수님이 추천해주신 영상입니다.

제가 작성한 내용은 실제 내용과 꽤 차이가 있을 수 있습니다. 직접 보시기를 권장합니다.


A Philosophy of Software Design | John Ousterhout | Talks at Google
https://youtu.be/bmSAYlu0NcY?si=G9KqjYZbOkMGMiHK


## Problem Decomposition
---
Computer Science에서 가장 중요한 콘셉트를 하나 고른다면 무엇을 고를 것인가?
- Abstraction
- Testing
- Complexity
- Layers of Abstraction (도널드 커누스)

화자가 말하기를 **Problem Decomposition** 이라고 합니다.

복잡한 문제를 어떻게 받아들이고 이를 상대적으로, 독립적으로 구축할 것인가? (relatively, independently)


## Can great programmer be taught?
---
우리는 10x 프로그래머 라고해서 평범한 개발자들 보다 10배 이상의 능률을 보이는 개발자가 있다고 말합니다.
왜 이러한 대단한 능력을 가르치려고 하지 않는걸까요? 심지어 가능하기나 할까요?

흔히 말해 생산성이 높은 대단한 프로그래머들은 재능이 있어서 스스로 깨우치고 성장한다고 생각합니다.
하지만 현실에서의 top 프로그래머와 average 프로그래머의 차이점은 **얼마나 연습했는가 (practiced)?**

이것이 유일한 consistent correlating factor (일관된 상관관계를 나타내는 요소) 입니다.
저는 (john ousterhout) 이를 가르칠 수 있다고 생각합니다.


## Shallow Class
---
![](/assets/images/Pasted%20image%2020231019030555.png)

비용: 모든 유저들이 알아야 하는 내용들 (Interface)
효과: 이 class를 사용함으로서 제공되는 기능의 유용함

모든 클래스는 Deep Class가 되어야 한다.

![](/assets/images/Pasted%20image%2020231019024848.png)