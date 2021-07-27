---
title: eclipse mariaDB jdbc driver가 작동하는 이유
categories: Java
tags: database jdbc driver
typora-root-url: ../
typora-copy-images-to: ../assets/images
---





`written by c4fiber`



여기서 왜 org.mariadb.jdbc.Driver가 바로 작동할까?

![image-20210524150434248](/assets/images/image-20210524150434248.png)



처음에는 jar파일을 추가하지 않은 상태여서 코드가 작동하지 않았다

![image-20210524150617129](/assets/images/image-20210524150617129.png)



추가하고 나니까 자동으로 됐는데 왜 jar파일만 추가했음에도 불구하고 알아서 작동했을까?



아무래도 connection jar파일 버전이 다르더라도 내부 패키지의 class명은 통일되어 있는것 같다 

- org.mariadb.jdbc 패키지에 Driver라는 class가 항상 있는거지





어찌보면 단순한 지식인데도 불구하고 난 몰랐네..

마음이 답답해진다.



그래도 의문을 해소한 기분이 들어서 좋기는 하네





## eclipse javafx sdk user library 추가하기

- 참고: https://stardevelop.tistory.com/2