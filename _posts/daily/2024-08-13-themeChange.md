---
layout: post
title: 블로그 테마 변경
subtitle: 그냥 유명한걸 쓰자
excerpt_image: /assets/images/posts/change.png
categories: React
tags: [React,Frontend,노마드코더]

---
![사진](/assets/images/posts/change.png)
### 블로그 테마 변경

# 계기

---

- 기존 쓰던 테마(jekyll-theme-satellite)가 로컬에서 EOF에러가 나고 테스팅을 할 수 없었다..
- 아무리 봐도 github에선 잘만 되는데 LF로 바꿔봐도 잘 안돼서 그냥 바꾸기로 했다… 좀 답답하기도 했고

# 테마 선정

---

- **JEKYLL YAT THEME**
    - 깔끔하고 년도별로 보기 좋았다. TOP페이지도 설정 가능해서 선택했다. 무엇보다 비교적 최근에도 계속 이슈를 수정하시는 듯 했고 기존 테마보다 fork수가 400배는 차이나서..
    - clone하고 난 뒤에 기존에 적었던 글을 테마의 구조에 맞게 수정했다.

## 추가 설정

1. config.yml에 remote_theme: "jeffreytse/jekyll-theme-yat”를 추가했다. 이후, 다른 설정을 수정했다. 
    1. 사실 이전에 여러 오류가 있었는데.. 내 생각엔 저걸 추가 안해서 그런듯 하다.. 
2. URI가 날짜로 잡혀서 permalink: /:categories/:title로 변경했다.
3. 사실 이미지가 없으면 default이미지가 아닌 글만 보여주려고 했는데 그건 잘 안됐다. ㅜㅜ 
4. scss에 가서 base font size를 키웠다. 
5. favicon이 추가가 잘 안되길래 head.html에 favicon을 추가했다.