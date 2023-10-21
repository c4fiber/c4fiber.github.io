---
title: Google Talk - John Ousterhout
aliases: 
tags:
  - Abstraction
  - SoftwareDesign
date created: 2023-10-19 02:48
---
## A Philosophy of Software Design | John Ousterhout | Talks at Google

pintOS 강의를 들으면서 권영진 카이스트 교수님이 추천해주신 영상입니다.

제가 작성한 내용은 실제 내용과 꽤 차이가 있을 수 있습니다. 직접 보시기를 권장합니다.


A Philosophy of Software Design | John Ousterhout | Talks at Google
https://youtu.be/bmSAYlu0NcY?si=G9KqjYZbOkMGMiHK

## Problem Decomposition

Computer Science에서 가장 중요한 콘셉트를 하나 고른다면 무엇을 고를 것인가?
- Abstraction
- Testing
- Complexity
- Layers of Abstraction (도널드 커누스)

화자가 말하기를 **Problem Decomposition** 이라고 합니다.

복잡한 문제를 어떻게 받아들이고 이를 상대적으로, 독립적으로 구축할 것인가? (relatively, independently)

## Can great programmer be taught?

우리는 10x 프로그래머 라고해서 평범한 개발자들 보다 10배 이상의 능률을 보이는 개발자가 있다고 말합니다.
왜 이러한 대단한 능력을 가르치려고 하지 않는걸까요? 심지어 가능하기나 할까요?

흔히 말해 생산성이 높은 대단한 프로그래머들은 재능이 있어서 스스로 깨우치고 성장한다고 생각합니다.
하지만 현실에서의 top 프로그래머와 average 프로그래머의 차이점은 **얼마나 연습했는가 (practiced)?**

이것이 유일한 consistent correlating factor (일관된 상관관계를 나타내는 요소) 입니다.
저는 (john ousterhout) 이를 가르칠 수 있다고 생각합니다.

## Shallow Class

![](/assets/images/Pasted%20image%2020231019030555.png)

비용: 모든 유저들이 알아야 하는 내용들 (Interface)
효과: 이 class를 사용함으로서 제공되는 기능의 유용함

모든 클래스는 Deep Class가 되어야 한다.

![](/assets/images/Pasted%20image%2020231019024848.png)

## Define Errors Out of Existence

![](/assets/images/Pasted%20image%2020231022045121.png)

사람들은 더 많은 예외상황을 처리하려고 노력한다. 내가 잘 막아내고 있는것처럼 생각한다.
하지만 예외상황이 많을 수록 더 많은 버그를 만들어내고, 연계되는 다른 예외상황을 만들어낼 수도 있다.

ex) Tcl unset: 변수를 삭제하는 명령어였다. 많은 사람들이 존재하지도 않는 변수를 삭제하려고 해서 exception으로 처리했다. 하지만 사람들은 지속적으로 이런행위를 반복했다.
내가 했어야 하는 일은 "redefine semantics"였다. unset 명령어를 변수를 "삭제"하는게 아니라 "사라지게" 만들었다. 기존에 없던 변수면 이미 없고, 존재하는 변수는 사라지게 만들면 된다.

ex) File Deletion: 이미 열려있는 파일을 삭제하려면 그 파일을 사용하고 있는 프로그램을 모두 종료시켜야 한다. 하지만 그 프로그램을 못찾는다면? 재부팅을 해도 시스템 데몬이 사용하고 있다면?
UNIX는 아름다운 방법을 사용했다. 파일을 삭제하면 namespace, directory에서만 삭제해서 파일시스템에서는 찾을 수 없게 했다. 마지막 프로그램이 파일을 읽기를 끝내면 그때 정리한다.


## Tactical vs Strategic Programming

![](/assets/images/Pasted%20image%2020231022050247.png)


