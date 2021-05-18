---
tile: "Docker commands (rpi4, armv7)"
date: 2021-05-18
categories: docker armv7 arm64 rpi rpi4
typora-root-url: ../
---



sudo usermod -aG docker $USER

출처: https://4urdev.tistory.com/80 [Simplify]



docker stop test

docker commit test

docker run --name test2 -p 8080:80 -d test



>  docker run -it -p [외부port]:[컨테이너 내부port] --name [컨테이너 이름] [image id] /bin/bash

doc

---

source: bash의 내부 명령어로 명렁어 뒤에 오는 파일을 읽어서 파일 내용 실행하는 역할.

출처: https://puham.tistory.com/23 [리눅스 SUPERUSER]



---

**~/.bash_aliases** 파일을 만들고, 아래 내용을 작성한다.

alias python=python3

alias pip=pip3

alias sudo='sudo '



출처: https://skylit.tistory.com/282 [초코아빠*]



---

virtualenv bcs

source bcs/bin/activate

deactivate

> 삭제명령어는 따로 없음 폴더 -r 옵션으로 지우기

pip freeze > requirements.txt





---

docker exec -it <container_id> /bin/bash

docker attach <container_id> = 도커 내부 쉘 접속 >> ubuntu 컨테이너에 이용



---

윈도우 > 리눅스 파일 전송하면 라인피드를 나타내는 글자가 달라서 에러발생함.

sed -i -e 's/\r$//' <target_file>

수행하면 사라짐

