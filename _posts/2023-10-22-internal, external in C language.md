---
title: 
aliases: 
tags: 
date created: 2023-10-22 20:56
---


## Storage-class specifiers

auto, register, static extern 같이 변수 앞에 붙는 선언(declarations)이다.
저장 클래스 지정자 라고 불린다.

https://en.cppreference.com/w/c/language/storage_duration



이 지정자가 존재하지 않는다면 기본적으로 다음과 같이 취급한다
- 모든 함수들: extern
- 파일 scope 안에 존재하는 객체들: extern
- 블록 scope 안에 존재하는 객체들: auto

### extern, static





```c
#include <stdio.h>
int main() {
	int x = 10;
	printf("x: %d\n", x);
	
	return 0;
}
```

같은 코드에서 `int x` 는 사실 `extern int x` 와 같은 의미이다.

그런데 왜 지역변수라고 하는걸까? 외부에서 접근하지 못하는 걸까?
이유는 block scope 때문이다.


