---
title: "Typora 이미지 경로 세팅 (github.io, offline)"
categ
ories: Editor
tags: typora github.io
typora-root-url: ../

---







c4fiber

실패링크 1

![image-20210519152934784](/../_posts/images/image-20210519152934784.png)



typora의 이미지경로: ./images

typora-root-url: ../assets/



새로운 이미지를 복사 붙여넣기하면 /../_posts/images/image-20210519152557099.png 이렇게 링크된다.

하지만 다음으로 이미지 복사를 통해 /assets/images/image-20210519152557099.png 에 이미지를 저장하면

typora-root-url의 영향을 받아 /images/image-20210519152937427.png 가 된다



즉 클립보드의 이미지는 typora 의 preference의 설정을 따라가지만, 그 이후의 저장 과정에서는 yaml의 typora-root-url의 영향을 받는다.

(첫 복사는 전역설정, 그 이후는 파일설정)





![image-20210519175449596](/assets/images/image-20210519175449596.png)



---

블로그 글을 쓴다고 할 때 사용된 자원(이미지, 등등)은 폴더별로 분류하는게 좋을까 한군데에 모으는게 좋을까?

사실 블로그의 게시글별로 폴더를 분리하면 시간이갈 수록 폴더가 어마어마하게 늘어나겠지. 그에 반해서 폴더 안에는 그닥 많은 데이터가 있지는 않을거야

반대로 생각해보면 하나의 폴더에 어마어마한 파일들이 모여서 문제가 생길 수도 있겠지

그럼 날짜에 기반해서 년도마다 파일들을 모아보면 어떨까?



지금당장은 보류하기로 했다. 지식도 부족하고 너무 많은시간을 할애할 수 는 없어서..

---

2021-05-19 17:46

아직도 실패했다! 성공한줄 알았는데..

typora image 경로는 ./assets/images

yaml 에 typora-root-url: ../

이렇게 설정하면 offline에서는 post 작성한 파일 하위에 이미지폴더가 생성된다.

---

2021-05-19 17:55

타협 완료

md 파일은 전부 github.io에 업로드하여 공개하도록 나만의 규칙을 만들었다.

이를 준수한다면 md파일을 위한 이미지는 내 github.io 의 assets에만 저장되면 된다



현재 설정

1. typora-preferece-image "when Insert - copy to custom foler"
2. 이미지를 저장할 github.io repository의 절대경로 입력
3. '가능하면 상대적 위치 사용' 옵션을 체크 (이러면 md파일 내에서 이미지 경로가 상대경로로 입력된다)
4. typora-root-url을 ../로 지정



이미지마다 특정 위치로 저장을 해주면 되지만 매번 그렇게 어떻게 하겠는가!!

반드시 상대경로를 이용해야된다는 나만의 생각을 바꾸게되는 계기가 되었다.



항상 절대적인 규칙이 존재할 수 는 없지



