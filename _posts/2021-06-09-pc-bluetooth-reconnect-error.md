---
title: PC bluetooth 연결 페어링됨 반복 오류 해결
category: Bluetooth
tag: bluetooth
typora-root-url: ../
---



`written by c4fiber`



연결상태: PC 블루투스 동글 - 갤럭시 버즈

잠시 내려놓았다가 재연결 하려고 하면 연결이 불가능합니다. 



이런 장면이 반복하게 되는데 해결방법을 제시합니다.

![image-20210609133919818](/assets/images/image-20210609133919818.png)



1. 작업관리자 열기

- 실행(Win + R) - devmgmt.msc

![image-20210609134025705](/assets/images/image-20210609134025705.png)

- 시작버튼 우클릭

  ![image-20210609134215086](/assets/images/image-20210609134215086.png)



2. Generic Bluetooth Radio -> 드라이버 업데이트

![image-20210609134420869](/assets/images/image-20210609134420869.png)



3. 내 컴퓨터에서 드라이버 찾아보기 -> ...직접선택

![image-20210609134525349](/assets/images/image-20210609134525349.png)

![image-20210609134559890](/assets/images/image-20210609134559890.png)



4. Generic Bluetooth Adapter 선택 후 다음 (더블 클릭)

   ![image-20210609134631581](/assets/images/image-20210609134631581.png)

   

5. 업데이트 완료 확인

![image-20210609141026512](/assets/images/image-20210609141026512.png)



6. 장치 제거 후 재연결

​	![image-20210609141804191](/assets/images/image-20210609141804191.png)



7. 성공!

![image-20210609141822643](/assets/images/image-20210609141822643.png)





무사히 오류 해결하시길 바랍니다.





