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

![](assets/images/Pasted%20image%2020231023194624.png)

static
- internal linkage, 내부적으로만 연결된다.
- 선언된 파일에서만 사용 가능하다.
- 서로 다른 파일에서 같은 이름으로 선언해도 영향을 미치지 않는다.
extern
- external linkage: 외부와 연결된다.
- 여러 소스파일에서 동일하게 사용할 수 있다.
- 서로 다른 파일에서 같은 이름으로 선언하면 충돌이 발생할 수 있다.
	- internal linkage로 동일한 이름으로의 선언이 있으면 internal linkage로 인식된다.
	- external linkage가 먼저 선언되었다면 external로 취급된다.

**둘 다 block scope** 내에 존재한다면 그 안에서만 유효하다.


