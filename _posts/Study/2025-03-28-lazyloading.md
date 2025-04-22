---
layout: post
title: 프론트엔드 성능 개선- lazy loading
subtitle: 필요할 때만 불러올까?
categories: Study
tags: [frontend, lazy loading]
---

자원(리소스)를 중요하지않다는 논 블로킹으로 식별해서 즉, 필요할 때만 로드하는 전략

이로 인해 HTML, CSS, Javascript를 화면에 픽셀로 변화하는 단계(중요 렌더링 경로 길이)를 줄여서 렌더링 성능을 향상시키고 페이지 로드시간을 감소 시킬 수 있다.

보통 스크롤, 내비게이션과 같이 사용자의 상호작용에서 발생.

## 전략들

### 1. 코드분할

HTML, CSS, Javascript를 더 작은 덩어리로 나눈 뒤, 요구되는 최소한 코드만 보내서 페이지 로드 시간을 개선하는 방법

1. 진입점 분할: 앱의 진입점에 따라서 분리
2. 동적 분할: dynamic import()문이 사용되는 코드 분리

### JavaScript

type=module이 있는 모든 스크립트 태그들은 JavaScript모듈로 취급되고 기본적으로 지연된다.

### CSS

css는 기본적으로 렌더링 리소스로 취급되기에 cssom이 구성될때까지 렌더링하지 않음. 따라서 렌더링 차단을 해제하는 게 좋다. css최적화를 위해 브라우저 네트워크 및 성능 도구로 어떤 부분이 시간이 오래걸리고 최적화 한지 분석하다.

1. 불필요한 스타일 시트 제거.
2. css를 별도 모듈로 분할. 로드시 필요한 css만 로드할 수 있도록 함. 이를 위해 css를 별도 파일로 분할하고 필요한 부분만 로딩하기.

```css
<!-- Loading and parsing mobile.css is not render-blocking on large screens -->
<link
  rel="stylesheet"href="mobile.css"media="screen and (max-width: 480px)" />
```

[미디어 쿼리를](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries/Using_media_queries)`media` 포함하는 속성을 추가해서 언제 적용해야하는 지 시점을 설정해 , 브라우저가 다운로드는 하지만 렌더링을 차단하지 않도록 한다.

1. css최소화, 압축: 파일의 모든 공백을 제거해서 최고화 하고, gzip과 같은 압축을 하는지 확인
2. 선택자 간소화: 선택자를 덜 복잡하고 구체적으로 만들어 구문 분석 시간을 줄이자. 유지관리에도 좋다.

```css
/* Very specific selector */
body div#main-content article.post h2.headline {
  font-size: 24px;
}

/* You probably only need this */
.headline {
  font-size: 24px;
}
```

1. 필요한 것만 스타일 적용: 범용 선택자와 같이 필요 이상의 요소에 스타일을 적용하지 않기. 특히 font-size와 같이 부모로부터 상속받는 속성은 적용하지 않도록 하기.
2. css sprite사용 : 여러개의 작은 이미지를 하나의 이미지 파일에 저장하고 각 이미지 파일에 다른 background-position을 사용해 원하는 이미지를 다른 위치에 표시할 수 있게 끔하는 sprite기술을 통해 http 요청을 줄이자
3. 중요 에셋 미리 로드하기: `rel="preload"` 로 중요 에셋을 미리 로드하도록 하자. 이를 통해 브라우저가 참조된 리소스를 빨리 가져와서 브라우저 캐시에 저장하기에 나중에 참조될때도 더 빨리 사용할 수 있다.
   사용자가 페이지 초반에 접하기 쉬운 리소스를 미리 로드해서 경험을 원활하게 만들자. (media 속성으로 반응형 프리로더를 만들 수도 있음)

### 애니메이션 처리

불필요한 애니메이션을 줄이자. 또한 특정 속성은 적용시 reflow(repaint)를 유발하기에 사용하지 않는게 좋다.

### **리플로우(Reflow)와 리페인트(Repaint)**

**reflow**
레이아웃에 영향을 주는 속성을 변경할 시, 브라우저가 전체, 일부의 DOM요소들의 위치와 크기를 계산하는 것.

**리플로우가 일어나는 상황**

- DOM 구조 변경 (ex: 노드 추가/삭제)
- 요소 크기/위치 변경 (`width`, `height`, `top`, `left` 등)
- 폰트 크기 변경
- 브라우저 창 크기 변경

**Repaint
스타일 변경으로 다시 화면에 그리는 것 ex) 색상, 배경색, 그림자와 같은 스타일 변경**

DOM 변경이나 스타일 변경 등 **레이아웃을 변경**하면 리플로우 발생→ 리플로우 후에는 다시 **리페인트**로 이어지며 성능 저하 유발할 수 있다.

따라서 다음과 같은 속성을 지양하는 게 좋다.

<aside>
❌

- 요소 크기 변경 예: `width`, `height`, `border`, `padding`

- 형태에 영향을 주는 시각적 효과 추가. 예: `box-shadow`

- 레이아웃 변경 예: `align-content`, `align-items`, `flex`

- 재배치 예: `margin`, `top`, `bottom`, `left`, `right`
</aside>

대신 다음을 사용해보자.

- Transforms
- `opacity`
- `filter`

### 폰트

폰트 요청은 렌더 트리가 구성될때까지 지연되기에 텍스트 렌더링이 지연될 수 있으므로 `<link rel="preload">`, `font-display, **CSS Font Loading API`, 등을 사용해서 리소스를 미리 로드해보자.\*\*

### 이미지와 Iframes

`<img>` 의 `loading` 속성이나 `iframe`의 `loading` 속성으로 필요할 때만 로드되게끔 할 수 있다.

```html
<img src="image.jpg" alt="..." loading="lazy" /><iframe src="video-player.html"
```

### **Intersection Observer API**

이 Intersection Obeserver로 브라우즈 뷰포트에 들어오고 나가는 시점을 사용자가 알게 만
