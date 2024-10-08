---
layout: post
title: 레이어드 아키텍쳐
subtitle: mvc, mvp, mvvm
excerpt_image: https://www.oreilly.com/api/v2/epubs/9781491971437/files/assets/sapr_0101.png
categories: Study
tags: [architecture, software-Architecture]

---

## 소프트웨어 아키텍처

- 소프트웨어 시스템의 구조와 디자인을 정의하는 개념
- 시스템을 구성하는 컴포넌트 간의 상호작용과 관계를 정의

## 중요성

1. 확장
    - 시스템의 규모가 커질 때 구조적 문제 발생을 줄이기 위해
2. 유지보수
    - 코드 수정 및 기능 추가로 시스템에 미치는 영향을 줄이기 위해
3. 성능
    - 성능 최적화를 위해
4. 보안
    - 잠재적 보안 취약점을 줄이기 위해

# layerd architecture
![mvc](https://www.oreilly.com/api/v2/epubs/9781491971437/files/assets/sapr_0101.png){: .align-center width=400 height=200}

## 설명

- 소프트웨어 아키텍처 중 하나로, 소프트웨어 시스템을 기능에 따라 여러 계층으로 분리하는 구조를 가짐
- 각 계층은 특정한 책임을 가짐
    - 단방향 의존성
        - 상위 계층은 하위에 의존 (상위가 하위에게 요청을 보냄)
        - 하위 계층은 상위에 대해 독립적 (즉, 하위는 상위에 대해 모름)

## 계층

1. **Presentation Layer** : 사용자 인터페이스(UI) 담당
    
    : 사용자 상호작용 처리, 사용자 입력 수집 등
    
2. **Business Layer** : 애플리케이션의 핵심 로직 처리 (데이터의 영속성 처리)
    
    : 재고 관리, 주문 처리 등 논리 관리
    
3. **Persistence Layer** : 데이터베이스와의 상호작용 처리
    
    : DB와 상호 작용하여 정보 검색, 정보 저장
    
4. **Database Layer** : 데이터 저장소 관리

## 장점

1. 모듈화, 분리
    - 각 계층이 독립적으로 개발, 유지 보수 가능
    - 코드 이해, 관리가 쉽고 책임이 명확하다.
2. 유지 보수가 쉬워짐
3. 테스트가 간편
4. 유연성
    - 구현 방식 변경 시 다른 계층과 독립적이므로 영향을 최소화

## 단점

1. 성능 저하 가능성
    - 요청이 각 계층을 걸쳐야 하므로 여러 계층을 걸치면 성능 저하
        - 데이터 교환, 캐스팅 시 부가적인 처리 과정 추가 가능성
2. 단방향 의존성으로인한 제약
    - 상위→하위로만 접근 가능하므로 기능 구현에 제약 가능성
    - 하위 계층의 변경이 상위 계층에 영향을 미치므로 유의

# 자주 쓰는 패턴

## 1. MVC
![mvc](https://media.licdn.com/dms/image/D5612AQHBlf7-k71W4g/article-cover_image-shrink_720_1280/0/1680876252211?e=2147483647&v=beta&t=tVrAgYVX6m7gXkbMtUKIOKH9WByhT570STp2YgZLV6Y){: .align-center width=300 height=200}

- 구성요소
    1. Model
        - 데이터와 비즈니스 로직 관리.
            - 데이터의 상태 유지, 데이터 처리 및 데이터베이스와의 상호작용 담당
    2. View
        - 사용자에게 보여지는 인터페이스 담당.
            - Model의 데이터를 화면에 표시하고, 사용자 입력을 받음
    3. Controller
        - 사용자 입력을 처리, 입력에 따른 Model, View를 업데이트 함.
            - 사용자 행동에 따라 데이터 조작, 결과를 View에 반영
- 작동방식
    1. 사용자가 View를 통해 입력 제공
    2. Controller는 이 입력을 받아서 처리하고, 필요한 경우 Model을 업데이트
    3. Model 상태가 변경되면 View가 이를 반영하여 사용자에게 출력
- 장점
    - 코드의 구조가 명확, 관심사의 분리가 잘 이루어짐
- 단점
    - Controller가 복잡해질 수 있고, 
    대규모 애플리케이션에서 View와 Controller 간의 의존성이 증가 가능성

## 2. MVP
![mvp](https://user-images.githubusercontent.com/52276038/85027902-692d6100-b1b5-11ea-8f9e-3a7c4eb19970.png){: .align-center width=300 height=200}

- 구성요소
    1. Model
        - 애플리케이션의 데이터와 비즈니스 로직 관리. 
        (MVC model과 동일)
            - 데이터의 상태 유지, 데이터 처리 및 데이터베이스와의 상호작용 담당
    2. View
        - 사용자에게 보여지는 인터페이스 담당. 
        MVC와 달리, View는 단순히 Presenter와의 상호작용을 통해 **데이터를 표시하는 역할만 수행**, **그 자체는 로직을 포함X**
    3. Presenter:
        - MVC의 Controller와 유사하지만, View와 Model 간의 상호작용을 **더 직접적으로 제어.**
            - View에서 발생한 이벤트 처리, Model 업데이트, 업데이트된 데이터를 View에 반영.
- 작동방식
    1. 사용자가 View에 상호 작용하면, View는 이를 Presenter에 전달
    2. Presenter는 Model을 업데이트하거나 데이터를 가져오고, View에 데이터를 표시할 방법 지시
    3. View는 Presenter의 지시에 따라 사용자에게 데이터 표시
- 장점
    - View와 비즈니스 로직이 철저히 분리되어 있어, View의 테스트가 쉬워짐
    - Presenter의 코드가 커질 수 있으며, 복잡한 로직이 집중될 수 있음.
- 단점
    - Controller가 복잡해질 수 있고, 
    대규모 애플리케이션에서 View와 Controller 간의 의존성이 증가 가능성 O

## 3. MVVM
![mvvm](https://upload.wikimedia.org/wikipedia/commons/thumb/8/87/MVVMPattern.png/500px-MVVMPattern.png){: .align-center width=600 height=200}


- **구성요소**
    1. **Model**
        - 애플리케이션의 데이터와 비즈니스 로직을 관리.
            - 데이터의 상태 유지, 데이터 처리 및 데이터베이스와의 상호작용 담당.
            - 다른 레이어와 독립적으로 동작하며, 도메인 로직을 캡슐화.
    2. **View**
        - 사용자에게 보여지는 인터페이스 담당.
            - 화면에 데이터를 표시하고 사용자의 입력을 받는 역할.
            - 로직을 포함하지 않으며, 단순히 ViewModel과의 데이터 바인딩을 통해 동작.
    3. **ViewModel**
        - View와 Model 간의 상호작용을 중개.
            - View의 상태를 관리하고, Model로부터 데이터를 받아와 View에 필요한 형태로 변환.
            - 커맨드, 데이터 바인딩 등을 사용하여 View와의 상호작용을 관리.
            - View와는 양방향 데이터 바인딩을 통해 실시간으로 데이터를 주고받음.
- **작동방식**
    1. 사용자가 View와 상호작용하면, View는 ViewModel에 바인딩된 속성들을 업데이트.
    2. ViewModel은 필요한 경우 Model을 업데이트하거나 데이터를 요청.
    3. Model의 데이터 변경 사항이 ViewModel을 통해 View에 반영되고, View는 이를 사용자에게 표시.
    4. View와 ViewModel은 양방향 데이터 바인딩을 통해 실시간으로 상호작용.
- **장점**
    - View와 Model 간의 강한 결합을 피하고, 코드의 모듈화를 통해 유지보수성이 높아짐.
    - ViewModel을 통한 데이터 바인딩으로 인해 View의 코드가 단순해지고 테스트가 용이해짐.
    - UI 코드와 비즈니스 로직이 분리되어, 개발자와 디자이너 간의 협업이 수월해짐.
- **단점**
    - 데이터 바인딩을 사용함에 따라, 데이터 흐름이 명시적이지 않아 디버깅이 어려울 수 있음.
    - 작은 프로젝트에서는 오버헤드가 될 수 있으며, 초기 설정과 구조화에 더 많은 시간이 소요될 수 있음.