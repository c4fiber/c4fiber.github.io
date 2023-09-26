---
title: 
aliases: 
tags: 
date created: 2023-09-26 17:22
---
Far pointer란 무엇인가?

## bing chat 답변

[Far 포인터는 주소를 두 개의 16비트 레지스터에 저장하는 포인터로, 현재 세그먼트 외부의 메모리에 접근할 수 있게 해줍니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/). [컴파일러는 세그먼트 주소를 저장하기 위해 세그먼트 레지스터를 할당하고, 현재 세그먼트 내의 오프셋을 저장하기 위해 또 다른 레지스터를 할당합니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/).

[오프셋은 이동된 세그먼트 주소에 추가되어 실제 주소를 얻습니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/). [Far 포인터에서 세그먼트 부분은 증가/감소로 변경할 수 없으며, 이는 오프셋만 변경되고 세그먼트 주소는 변경되지 않기 때문입니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/). [Far 포인터의 크기는 4바이트입니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/).

Far 포인터의 문제점은 포인터가 다른 값을 가지지만 같은 주소를 가리킬 수 있다는 것입니다. [따라서, 포인터 비교는 Far 포인터에서 무의미합니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/).

[Far 포인터의 C 언어 구문은 다음과 같습니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/):

```c
pointer_type far * pointer_name;
```

[예를 들어, Far 포인터를 사용하는 C 코드는 다음과 같습니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/):

```c
#include <stdio.h>
int main () {
    int far* ptr;
    printf("Size of Far Pointer: %d bytes", sizeof(ptr));
    return 0;
}
```

[이 코드의 출력은 "Size of Far Pointer: 4 bytes"입니다](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/)[1](https://www.geeksforgeeks.org/what-are-near-far-and-huge-pointers/).

## 16-bit intel architectures

레지스터의 크기는 16bit 이지만 주소 버스(address bus)의 크기는 20bit 일때 사용되던 기술이다.

16bit 레지스터에 세그먼트의 주소를 far pointer(4bytes)에 저장하고 오프셋(offset)을 near pointer(2bytes)에 저장해서 20bit 주소 크기를 참조할 수 있도록 한 방법이다.

현재는 사용되지 않는다.

