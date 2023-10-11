---
title: 
aliases: 
tags: 
date created: 2023-10-11 22:06
---
## Supplemental Page Table

보조 페이지. 각 페이지에 대한 추가적인 정보를 포함하고 있다.
그냥 page table을 바로 수정하면 안되는가?
페이지 테이블의 포맷의 제한이 있다.
- spt_find_page: find struct page from spt and virtual address
	- page에 대한 struct page를 찾는다. spt, virtual address를 기반으로
	- spt 혹은 virtual address 정보를 기반으로 관련 정보를 찾아내는 함수


두가지 목적이 존재한다.
- 페이지 폴트가 발생하면 커널은 SPT에서 가상 페이지(virtual page)를 찾는다. 어떤, 어디에 있는 정보가 존재하는지 확인한다.
- 프로세스가 종료되면 커널이 어떤 페이지를 free 시킬지 정보를 얻는다.


