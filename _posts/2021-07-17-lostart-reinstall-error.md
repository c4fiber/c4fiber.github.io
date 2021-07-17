---
title: 로스트아크 재설치 오류 해결
categories: etc
typora-root-url: ../
---

`written by c4fiber`



지정된 장치, 경로 또는 파일에 액세스 할 수 없습니다. 이 항목이 액세스할 수 있는 권한이 없는 것 같습니다.

해당 경로로 들어가면 LostArk.exe 파일이 없다.



![stove 01](/assets/images/stove 01-1626517933974.PNG)





게임파일을 모두 삭제하고 다시 설치를 진행한다.



![stove 02](/assets/images/stove 02-1626517932014.PNG)





원인은 백신 프로그램에 있다. 본인은 카스퍼스키 FREE 버전 사용중



![stove 03](/assets/images/stove 03-1626517930325.PNG)





리포트를 확인해보니 STOVE.exe, LostArk.exe 파일을 악성파일로 인식하여 삭제함



![stove 04](/assets/images/stove 04-1626517920966.PNG)





예외규칙을 설정해주면 해결.

STOVE.exe에서도 발생하므로 상위폴더인 Smilegate 전체를 예외처리 하였다. (권장하지 않음)



![image-20210717192738928](/assets/images/image-20210717192738928-1626517923633.png)





즐거운 게임 합시다!



