---
layout: post
title: 성능 분석 방법 (chrome)
subtitle: 크롬으로 성능 분석하기기
categories: Frontend
tags: [frontend, performance]
---

## JavaScript 성능 분석

병목된 사항을 JavaScript 프로파일러가 알려준다.

Firefox의 개발자 도구, Chrome 도구도 있다!

### 1. Performance 탭

- 페이지 렌더링이 느리거나, JS 애니메이션이 끊기거나, 스크롤이 버벅이거나 반응이 느릴때 사용한다.
- 사용 방법
  1. `Record` 버튼 클릭 후, 페이지에 문제 있는 동작 수행
  2. 중지하면 타임라인 분석 가능
- 주요 정보
  - **Main Thread 활동**: JavaScript 실행, 레이아웃 계산, 페인트 등
  - **Frames**: 초당 프레임 (FPS) 확인 가능 (60fps에 가까우면 OK)
  - **JS 함수별 소요 시간**도 나옴 (병목 함수 파악 가능)

### 2. Network 탭

- HTML, JS, 이미지 등 리소스 로딩 속도가 느릴 때
- 파일 크기, 응답 속도, 캐싱 확인
- 주요 기능
  - 각 요청 크기/ 소요 시간/ 상태 코드 확인
  - Waterfall 차트로 순서와 병렬 요청 여부 파악
  - Throttle 기능: 모바일 3G/4G 네트워크 속도로 시뮬레이션 가능

### 3. Lighthouse 탭 (자동 분석 리포트)

- 전체 성능, 접근성, SEO, PWA평가 리포트 받을 때
- 주요 기능
  - “Generate report” → 페이지 분석, 점수 평가
  - **First Contentful Paint**, **Time to Interactive** 등 주요 지표 제공
  - 개선사항 추천도 함께 제공됨 (예: 이미지 최적화, 캐싱 추가 등)

### 4. Memory 탭

- 메모리 누수, 불필요한 오브젝트 확인
- 주요 기능
  - Heap snapshot : 메모리 사용 현황 시각화
  - **Allocation instrumentation**: 어느 시점에 메모리가 얼마나 사용됐는지 확인
  - Garbage Collection(GC)이 잘 일어나는지 체크 가능

### Application 탭

- LocalStorage, IndexedDB, 쿠키, 캐시 사용 확인 및 디버깅
- PWA 관련 리소스 분석.

| 상황                    | 도구                                       |
| ----------------------- | ------------------------------------------ |
| 페이지 반응이 느릴 때   | `Performance` 탭으로 FPS, JS 실행시간 확인 |
| 로딩이 느릴 때          | `Network` 탭으로 요청 시간 분석            |
| 전반적인 웹 성능 점검   | `Lighthouse` 리포트 생성                   |
| 메모리가 계속 증가할 때 | `Memory` 탭으로 누수 확인                  |
