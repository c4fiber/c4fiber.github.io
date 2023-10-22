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

내가 생각하기로는 소프트웨어 디자인에서 중요한 부분은 무엇이 중요한지, 중요하지 않은지를 찾는것이다.
이상적으로는 가능한 작은 부분이 중요하게 여겨져야한다. 정말 중요하고 문제가 되는 부분을 찾아서 당신의 시스템에 반영해야 한다.

Exceptions vs Return Value
- stack에서 멀리 보내려면 exception
- 한단계씩 보내려면 return value

## Tactical vs Strategic Programming

![](/assets/images/Pasted%20image%2020231022050247.png)

내가 생각하기에는 좋은 디자인의 가장 큰 장애물은 마인드셋이다.
마인드셋 없이는 절대 좋은 디자인을 내놓을 수 없다.

대부분은 전술적 접근을 한다.
- 목표: 다음 기능, 버그 고치기 
문제는 최대한 깔끔하게 만들겠지만 몇가지 숏컷을 만들고.. kluges 만들고... 임시방편을 만들고...
팀의 모든 사람들이 이러한 형태를 갖추게 된다면 이를 고치는데 수맣은 시간과 노력이 들어갈 것이고 매우 빠르게 스파게티 코드로 변화할 것이다.

문제는 complexity는 한 하나의 실수때문에 발생하는게 아니라 수많은 실수로 인해서 발생한다.
물론 시간을 들여서 처리할 수 있겠지만 결국은 압도당해서 절대 하지 않게 될 것이다.

Tactical Tornado: 혼자서 수많은(약 80%만 작동하는) 저품질의 코드를 생산하는 사람을 말한다. 그리고는 wake of destruction을 그 뒤에 남긴다.
대부분의 조직에서는 이러한 사람들이 영웅으로 받아들여진다. 겨우 내일까지 작동하는? 심지어 이런사람들이 10x 프로그래머라고 생각하는 사람도 있다!

이처럼 tactical한 접근은 매우 빠져들기 쉬우며 그러지 않기 쉽지않다.
좋은 디자인을 하기 위해서는 작동하는 코드로는 충분하지 않다는걸 깨닭아야 한다.
작동하는 코드는 단 하나의 목표가 될 수 없다. not single goal, table stakes

## cont'd (continued)

![](assets/images/Pasted%20image%2020231022055158.png)

you have to invest. 내 의견으로는 결국 다 돌아오게 된다.

초반엔 얼마나 천천히 가야하는가? 언제서야 tactical한 접근을 따라잡을 수 있나?
내 의견으로는 왜 이러한 접근을 해야하는지 깨닭고 난 후 6 ~ 12개월정도 걸린다고 생각한다.

## How Much To Invest?

![](assets/images/Pasted%20image%2020231022055328.png)

망가진, crappy한 코드로도 성공은 할 수 있다.
하지만 그러지 않고도 성공할 수 있다. Google, VMware는 그러지 않고도 성공했다.
이런 좋은 문화를 가지고 있다면 최고의 프로그래머들을 끌어모으는데 꽤 좋은 위치에 서있게 될 것이다.
우리는 10x 현상을 알고있다. 더 좋은 제품을, 더 빠르게 만들고 제공하기 위해서는 최고의 프로그래머들을 영입하는 것이다. 그래서 좋은 디자인 문화를 강하게 지지한다면 최고의 인재르 고용할 수 있게 해준다고 생각한다.
(I think the strongest argumentin favor of a good design culture is that it allows you to hire top people)

얼마나 투자할 것인가? 내가 다시 물어보자면 얼마나 투자할 수 있는가? 너 자신에게 물어라 "나는 내 인생에서 이 단계에 얼마나 투자할 수 있는가?" 나는 대략 10% ~ 20% 라고 생각한다.

적어도 10%는 투자할 수 있을 것이다. 매몰비용이 아니라 반드시 돌아온다.
영웅적인 큰 투자가 아니라 조금씩의 꾸준한 투자. 모든 시스템을 완전히 디자인하기위해 6개월을 투자할 수는 없다. 조금씩 점진적으로 투자해야한다.

그래서 모듈을 새로 만든다면 인터페이스를 설계할 때 조금의 시간을 투자하고 deep classes와 함께하도록 시도해라. 어떻게 진행되는지 문서로 작성하고 , 단위테스트도 물론이다.

처음엔 잘 모를것이다. 제대로 안되는게 당연하다. 그게 소프트웨어의 룰이다. 항상 무언가를 발전시키고, 증진시킨다고 생각하라. 항상 더 좋게 만들 수 있도록 살펴봐라.

이러한 이유중에 첫번째는 당신은 아마도 무언가를 만들때 망치게 될 것이다. 그래서 이를 적어도 반반으로 만들고 싶으면 무언가를 발전시켜야 한다. (ㅋㅋ)
(One reason for this is you're brobably making something worse when you go in also. So even if you just want to break even, you've got to find something to improve)

그래서 나는 그저 반반으로 만들려고 노력하는 것이라고 생각하는 편이다.
일반적으로는 사람들이 존재하는 코드에서 개선사항을 찾을때, 가장 적은줄의 코드로 변경할 수 있는 가능성을 찾는다. 내가 생각하기엔 그들은 그저 겁먹은 거라고 생각한다. (난 이해하지 못하고, 뭔가 부숴버릴것 같고, 그래서 global variables 들을 사용해서 점프하고..)

그러지 마라. 뭔가 하기
