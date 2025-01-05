---
layout: post
title: CRA create-react-app typescript 오류
subtitle: react19 error, npm
excerpt_image: https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb2G0T4%2Fbtryy2p6DPA%2FBPgIlizCBiqdGZQupFFL51%2Fimg.png
categories: Error
tags: [React, npm, CRA, Vite, Build]
---

### 오류 상황

```bash
npx create-react-app my-app --template typescript
```

이렇게 react app 초기 셋팅을 CRA를 통해 했으나 오류가 발생했다.

```bash
npm install --no-audit --save @testing-library/jest-dom@^5.14.1 @testing-library/react@^13.0.0 @testing-library/user-event@^13.2.1 @types/jest@^27.0.1 @types/node@^16.7.13 @types/react@^18.0.0 @types/react-dom@^18.0.0 typescript@^4.4.2 web-vitals@^2.1.0 failed
```

React 19 버전과의 의존성 문제인 것 같아 찾아보니 react 19로 업데이트 되면서 오류가 발생하는 것 같다.

### 해결 방법

### 1. yarn 사용하기

yarn은 react와 같은 프로젝트를 진행하면서 어려움을 겪었던 걸 보완하기 위해 개발되었기에 npm보다 속도, 안정성 측면에서 더 우수하고 관리하기 편하다.

```bash
yarn create react-app first-app --template typescript
```

기본적인 yarn 명령어는 다음과 같다.

- yarn `add` {module_name}
- yarn `remove` {module_name}
- yarn `upgrade`
- yarn `remove` {module_name}

### 2. [Vite 사용하기](https://ko.vite.dev/guide/)

Vite(/vit/)는 최근 주목 받고 있는 빌드 도구이다. 여러 템플릿을 지원하고 있어서 자신이 원하는 템플릿을 사용하면 된다. ~~발음은 바이트가 아니라 빗…~~

- `vanilla`, `vanilla-ts`, `vue`, `vue-ts`, `react`, `react-ts`, `react-swc`, `react-swc-ts`, `preact`, `preact-ts`, `lit`, `lit-ts`, `svelte`, `svelte-ts`, `solid`, `solid-ts`, `qwik`, `qwik-ts`

```bash
npm create vite@latest
or
npm create vite@latest my-vue-app -- --template react-ts
```

- CRA:초기 설정,구성을 자동화 하여 빠르게 react app을 생성하도록 도와줌.
  - HMR 은 저장하면 바로 화면에 적용해주는 기술
  - webpack을 통해 빌드 시에 코드를 컴파일하고 압축해주는 역할. 왜냐하면 typescript 자체 코드로 돌아가지 않기 때문에 javaScript로 컴파일 해주고 압축, 번들링하게 됨.
  - prcess.env.KEY는 환경변수 설정
- Vite: 빠른 속도 효율로 최근에 주목 받고 있음. ESbuild를 사용해서 wepack대신 rollup으로 빌드를 함. rollup은 webpack에서 속도가 개선된 버전이며(아마도) 개발모드인 HMR에서 빠르다.
  - 왜냐하면 CRA는 HMR에서 소스 전체를 빌드하는 반면, Vite는 모듈 단위로 빌드하고 브라우저에 제공하기에 속도 차이가 난다.
  - import.mea.env.KEY라는 독특한? 자신들만의 방식을 사용한다.

### CRA로 React App 초기 설정하기

```powershell
npx create-react-app book-store --template typescript
```

### Vite로 React App 초기 설정하기

```powershell
npm create vite@latest book-store -- --template react-ts
```

지원하는 버전이 상이하면 nvm으로 버전 체인지를 할 수 있다.

```powershell
nvm install 18.21.1
```

<aside>
🚫

Failed to remove some directories가 날 때는

1. node_modules 삭제 → 2. package-lock.json 삭제→ 3. : "npm install"

</aside>
