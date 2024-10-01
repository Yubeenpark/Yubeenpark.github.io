---
layout: post
title:  JavaScript와 비동기
subtitle: 싱글 스레드, 비동기 처리
categories: Study
tags: [비동기, JavaScript, Thread]
excerpt_image: https://upload.wikimedia.org/wikipedia/commons/thumb/9/99/Unofficial_JavaScript_logo_2.svg/512px-Unofficial_JavaScript_logo_2.svg.png?20141107110902
---

# JavaScript

---

## 1. JavaScript 엔진

- Call Stack과 Memory Heap으로 이루어짐.
    - Call stack: **`원시`** 타입 데이터 저장, 코드 실행 순서 관리 LIFO
    - Memory Heap: 객체 등의 **`참조`**타입의 데이터가 저장됨. 메모리 할당이 일어남.
- 단일 호출 스택을 사용하기에 싱글 스레드다.
- 동기적 함수들은 Call Stack으로 처리. 그러나 Promise,DOM과 같은 비동기 함수는..?

<aside>
❓

원시 타입

- 하나의 값을 담은 자료형 ex) null, number, string…
- 하나의 메모리에 하나의 데이터 보관

참조 타입

- 동적 데이터를 담은 자료형 ex) 배열, 객체 함수
- 변수 할당 시, **주소** 저장
</aside>

## 2. Javscript의 비동기 함수 처리

### Web API

- 웹 브라우저에서 제공 하는 API로 DOM, Timeout,AJAX등이 있음.
- 비동기 함수를 처리하고, 그동안 Call Stack은 나머지 동기 함수들을 처리함.

### Event Loop

- 스레드 안에 있는 코드를 스케줄링하고 실행시켜 Javscript엔진(single thread)을 멀티 스레드인 구동 환경과 연결시켜주는 역할.
- Call Stack에 함수가 존재하지 않을 때, 우선 순위에 따라 FIFO로 task queue에 있는 비동기 함수를 Call Stack에 넣음.

### task Queue

- 비동기 함수를 보관하는 장소. Event Loop에서 함수 꺼내기 전까지는 계속 Queue방식으로 보관함.
    1. Macrotask
        - MicroTask를 싹 다 비운 뒤에 MacroTask를 처리함.
    2. Microtask 
        - promise로 만듦. then, cathc, finally, await 핸들러

https://www.jsv9000.app/

위 링크에서 작동 방식을 보면 이해가 빨리 된다.