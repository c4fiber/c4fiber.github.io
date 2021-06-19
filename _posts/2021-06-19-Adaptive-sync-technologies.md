---
title: vsync, freesync, gsync compatible, gsync 비교
categories: graphics
tags: vsync freesync gsync_compatible gsync monitor
typora-root-url: ../

---

`written by c4fiber`



24GN600 모니터를 산지 한달? 정도 되는것같다.

Apex legends, LOL을 플레이하면서 Gsync compatible을 사용하고 있는데 더 나은 환경을 위해 조사했던 기록을 남긴다.



목표

1. 수직동기화란
2. adaptive sync
3. VESA Adaptive sync
4. freesync
5. g sync
6. g sync compatible
7. 나에게 최적화된 설정은?



## 1. V Sync (수직동기화)

수직동기 버퍼를 이용해 FPS(frame per second) = refresh rate(주사율)을 구현하는 기술

144hz 모니터라면 수직동기 버퍼에 144frame을 집어넣고 모니터로 전달한다. 딜레이 발생.

144frame이 안나오면 72frame으로 줄여서 72frame만 전달함. 순간적인 인풋랙 발생.

실시간으로 출력 프레임이 변화하는 GPU의 상태를 최대로 끌어올리기 어렵다.



장점: tearing(티어링 tearing, 화면 찢어짐 현상) 감소

단점: input lag (인풋랙) 증가. 화면출력 딜레이 발생.



## 2. Adaptvie Sync (어댑티브 싱크)

화면 주사율(Refresh rate)를 프레임(FPS)에 맞추는 기술을 뜻하는 용어. 

VRR(Variable Refresh Rate) 기술이다.

프레임을 화면의 고정주사율과 맞추는 수직동기화와는 반대로, 가변적인 화면 주사율을 GPU의 프레임와 맞춰준다.



수직동기화: GPU에서 전달받은 프레임을 주사율에 맞춤

Adaptive Sync: GPU가 만든 Frame에 맞춰 주사율을 변경함.



## 3. DisplayPort Adaptive Sync (디스플레이포트 어댑티브 싱크)

VESA에서 발표한 VRR(Variable Refresh Rate)기술.

DisplayPort(DP) 포트를 사용할 경우에만 작동. Freesync와 G sync compatible의 기반 기술이다.





## 4. Freesync (프리싱크)

AMD에거 발표한 Adaptive Sync 기술. 

DisplayPort Adaptive Sync 의 표준을 준수하며 AMD 그래픽카드(라데온)에서 작동한다.

HDMI로도 작동할 수 있는게 특징



## 5. G Sync (지싱크)

NVIDIA에서 발표한 VRR 기술.

DisplayPort Adaptive Sync와는 다르다.

하드웨어적으로 작동하기 때문에 G Sync module이 필요하다.

지싱크 모듈이 장착된 모니터 + NVIDIA Graphic Cards 조합으로만 사용할 수 있다.

모든 G Sync 모니터는 NVIDIA에서 테스트 후 인증한다.

https://www.nvidia.com/ko-kr/geforce/products/g-sync-monitors/specs/



## 6. G Sync Compatible (지싱크 호환)

NVIDIA에서 발표한 Adaptive Sync 기술.

근본적으로 G Sync와 다르며 Display Adaptive Sync 표준을 준수한다.

때문에 Freesync 모니터 + DP port 조합으로 사용 가능하다. 





## 7. 나에게 최적화된 설정

Freesync를 지원하는 24GN600 모니터 + RTX 3070

나는 G Sync Compatible을 사용가능하며 실제로도 사용중이다.



이 게시글을 작성하는 이유는 혹시라도 지싱크 호환이 끄는 것보다 좋을지 고민했었다.

근데 부질없었다. 나는 화면 티어링을 막는게 먼저이다.

알면서 궁금했다. 알고나면 두려움이 덜해지니까.



결론은 G sync Compatible 사용하자!



## 번외 1: 티어링이 생기는 이유?

화면 주사율이 120hz라고 하자. 즉 1초에 120번을 깜빡인다는 뜻이므로, 1초에 이미지 120장을 보여줄 수 있다는 뜻이다.

그런데 GPU가 실시간으로 만들어내는 이미지(Frame)은 120장으로 고정되지 않고 매번 유동적이다.



이해하기 쉽게 하나의 이야기로 풀어보겠다.

1. 나는 게시판에 안내문을 게시해야한다.
2. 게시판은 60개의 안내문을 게시할 수 있다.
3. 선생님은 나에게 61개의 안내문을 주었다.
4. 나는 일단 모든 안내문을 걸어야 하므로 60장을 걸고 한장을 어딘가에 겹쳐서 게시하였다.
5. 안내문은 투명종이에 적혀있어서 겹친 종이는 서로다른 내용이 겹쳐보이게 된다. 

