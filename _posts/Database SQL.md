---
title: "Database SQL"
date: 2020-05-18
categories: Database
---



title: "Welcome to Jekyll!"
date: 2020-11-29
categories: jekyll update



create table tb1 (
    studentID char(9) constraint pk_tb1 primary key,
    studentName varchar2(30) NOT NULL,
    grade number(1) check(grade >= 1 AND grade <= 4)
);


check를 사용하면 not null조건을 주지 않아도 되는가?
=> 주어야 한다. null은 비교자체가 안되기 때문에 check로 검사를 할 수 가 없다. -> 바이패스가 된다.