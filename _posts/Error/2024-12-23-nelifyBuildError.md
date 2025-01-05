---
layout: post
title: nelify 빌드 오류
subtitle: 대소문자
excerpt_image: https://searchvectorlogo.com/wp-content/uploads/2023/06/netlify-logo-vector-2023.png
categories: Error
tags: [Error, nelify, build]
---

### 오류 상황

특정 파일(import 했던) 을 찾을 수 없다고 떠서 확인했지만 대소문자도 완벽하게 잘 맞아서 왜 오류가 발생했는지 몰랐다. 알고보니 nelify에 이전에 올린 파일 명과 현재 올린 파일 명이 달라지면 계속 이전 것에 영향을 받는 듯 하다.

즉, 원래는 button.tsx라고 올렸는데 갑자기 Button.tsx로 하고 import 할때 주소도 다 변경해도 이전 값이 nelify에 영향을 줘서 혼동을 야기하는 것 같다.

### 오류 해결

파일 삭제한 뒤 git commit→ git push 하고 다시 생성한 뒤 git commit→ git push하면 해결 된다.
