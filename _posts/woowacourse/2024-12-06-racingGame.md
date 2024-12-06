---
layout: post
title: [7기]우테코 프리코스 코드 리팩토링
subtitle: <javascript> 2주차 racingCar
excerpt_image: https://oopy.lazyrockets.com/api/v2/notion/image?src=https%3A%2F%2Fprod-files-secure.s3.us-west-2.amazonaws.com%2Ff71cbdcd-b763-41af-9bbb-42abdb18bd6a%2F9a492b46-973c-4d63-af79-b948e97b2391%2F%25E1%2584%258B%25E1%2585%25AE%25E1%2584%258B%25E1%2585%25A1%25E1%2584%2592%25E1%2585%25A1%25E1%2586%25AB%25E1%2584%2590%25E1%2585%25A6%25E1%2584%258F%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25A9%25E1%2584%2589%25E1%2585%25B3-2025-%25E1%2584%2589%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%258B%25E1%2585%25B5%25E1%2586%25B8%25E1%2584%2589%25E1%2585%25A2%25E1%2586%25BC-%25E1%2584%2586%25E1%2585%25A9%25E1%2584%258C%25E1%2585%25B5%25E1%2586%25B8-%25E1%2584%2591%25E1%2585%25A9%25E1%2584%2589%25E1%2585%25B3%25E1%2584%2590%25E1%2585%25A5_outline.png&blockId=8e79f541-d498-4c2a-9e85-2f60c433a990
categories: Study
tags: [woowacourse,우테코]
---
# I. 나의 코드

https://github.com/Yubeenpark/javascript-racingcar-7

사실 3주차까지 무난했지만 4주차에 폭격을 맞아 많이.. 힘들었다. 

왜냐하면 객체지향스럽게 코드를 짜본 적이 많이 없기 때문이다.

그저 코딩 테스트 통과를 위하여 효율적 이거나 알고리즘을 많이 사용했기에 더더욱 어색했다. 지금은 최종 코테를 대비하면서 내가 썼던 코드를 다시 예쁘게 바꿔보려고 한다. 

참고로 원본은 여기다. 

https://github.com/woowacourse-precourse/javascript-racingcar-7

## 객체 지향 (Object-Oriented Programming, OOP)

정리하면 1. 공통 특성을 묶자! 2. 속성 값(변수)과 행위(함수)를 묶어보자.  이게 가장 큰 특징인 것 같다. 

- 도서 관리도 반납하기, 대출하기를 페이지 수, 제목과 같은 자료형 필드와 같이 묶어서 관리 가능하다.
    - 함수 코드를 반복해서 안써도 되니 중복 코드도 줄어들고, 독립성도 서로 생기고 좋다.

 또한 이와 반대되는 개념인 절차 지향을 생각해보면 더 이해가 빠르다. 절차 지향은 건축과 비슷해서 `완벽`한 설계, 순서를 지켜야 하는데 코드는 99퍼센트로 수정, 추가, 삭제 등 중간에 리터치가 많이많이 들어간다. 
따라서 완벽한 설계는 첨부터 불.가.능!

유명한 특징부터 같이 봐보자. 

### 1. 추상화(**공통 특징** 추출)

 ex) 자동차, 오토바이- 추상화는 이동 수단

### 2. 캡슐화(객체 내부 구현을 외부로부터 **감추기**, **private 사용**, 속성(변수), 행위(함수)를 묶는 과정)

### 3. 상속(부모 기능 물려받고 수정하여 사용. 공통 특성을 부각시켜 하나의 개념으로 성립, 자식과 부모, 기존 클래스 재사용하여 새로운 클래스를 작성.

<aside>
💡

함수 중복, 기능 중복이 왜 별로일까?

만약 기능을 살짝 바꾸거나 추가해야할 때, 그 모든 코드에 다 복붙해야한다…오 이런..

**절차지향, 건물 쌓기 같이 바뀌지 않는 다는 가정이 아닌, 코드는 수정을 반복하는 객체 지향… 

</aside>

### **다형성**(**수정과 추가**, 맥락에 따라 다른 역할을 수행할 수 있게 하는 것)

- 오버라이딩(ride): 메소드와 같은 이름, 배개변수를 재 정의 (덮어야 하므로 다 같아야함)
- 오버로딩(load): 이름 같지만 전달인자가 다르다.  같은 이름 함수를 여러 개 정의 후, 매개변수 타입, 개수를 다르게 해서 매개변수 따라 호출하는 것. (그냥 이름만 불러옴)

# II. 프리코스 이후 나름 OOP와 RSP를 지키며 리팩토링

## 1) UI 로직, 비즈니스 로직 나누기.

### UI 로직 (InputValue, OutputValue)

UI는 InputValue와 OutputValue로 나눴다. 

Input은 InputMessage를 필드로 갖고 있으며,  생성자의 매개변수로 초기화한다.

Output은 static 메소드를 가지고 있고, 매개변수로 출력할 값을 전달하면 된다.

```jsx
import { MissionUtils } from "@woowacourse/mission-utils";
const { Console } = MissionUtils;

export class InputValue {
  inputMessage;
  constructor(inputMessage) {
    this.inputMessage = inputMessage;
  }

  async getUserInput() {
    return await Console.readLineAsync(this.inputMessage);
  }
}
export class OutputValue {
  static printOutput(outputMessage) {
    Console.print(outputMessage);
  }
}
```

### 비즈니스 로직(RacingGame, Car)

 각 차의 인스턴스를 만들 수 있는 Car class와 게임을 관리하는 RacingGame class를 만들었다.

### a. Car class

차 이름을 생성자의 매개변수로 전해줘서 생성한다.

 각 차의 이동 거리를 담기 위한 distance도 가지고 있다. 각 차가 이름, distance 각자 갖고 있기 때문에 class로 관리해줘야 한다고 생각했기 때문이다.

 또한, Car의 필드를 다른 곳에 쉽게 넘겨주면 안되기에 출력을 위한 getDriver만 빼고 distance와 같은 경우, 다른 Car와 비교하는 메소드를 통해 최댓값을 알아낼 수 있도록 구현했다. 

```jsx
export class Car {
  #driver;
  #distance;

  constructor(driver) {
    this.#validate(driver);
    this.#driver = driver;
    this.#distance = 0;
  }

  getDriver() {
    return `${this.#driver}`;
  }

  #validate(driver) {
    if (driver.length > 5) {
      throw new Error("[ERROR] 자동차 이름은 5자 이하여야 합니다.");
    }
    if (driver === "") {
      throw new Error("[ERROR] 자동차 이름이 없습니다.");
    }
  }
  moveForwardShowPositions(canMove) {
    this.moveForward(canMove);
    this.viewPosition();
  }
  moveForward(canMove) {
    if (canMove) {
      this.#distance++;
    }
  }

  compareDifferenceDistance(otherCar) {
    return this.#distance - otherCar.#distance;
  }
  isSameDistance(otherCar) {
    return this.#distance === otherCar.#distance;
  }
  viewPosition() {
    const view = this.#driver + " : " + "-".repeat(this.#distance);
    OutputValue.printOutput(view);
  }
}

```

### b. RacingGame

 게임에 참가하는 car들의 이름들과 게임 횟수를 가지고 있다.

  여기서 racingGame이 한번 더 실행되었을 때 추가 실행이 가능하도록 생성자가 아닌 setGameRound()로 초기화하도록 했다. 또한 입력 값에 관한 `유효성 검사`도 racingGame과 Car내에서 진행하도록 했다. 왜냐하면 UI로직은 단순히 사용자에게 보여주는 곳이므로 유효성 검사는 `비즈니스 로직`에서 다뤄야 한다고 생각했기 때문이다. 

 초반에는 RacingGame에서 input의 메소드 조차 불러오면 안된다고 생각했었다. 그게 분리라고 생각했지만, 실제 기능을 중점으로 보면 Input에서 담당하고 있기에 RacingGame 내에서 input class를 통해 사용자 입력을 받은 뒤, 유효성 검사를 진행하도록 변경했다. 또한, 올바른 값이 입력될 때 까지 반복하도록 만들었다. 

```jsx

export class RacingGame {
  #cars;
  #round;

  async setGameCars() {
    const input = new InputValue(
      "경주할 자동차 이름을 입력하세요.(이름은 쉼표(,) 기준으로 구분)"
    );
    const cars = await this.getInputCheckValidation(input, this.validateCars);
    this.#cars = cars.map((car) => new Car(car));
    return;
  }

  async setGameRound() {
    const input = new InputValue("시도할 횟수는 몇 회인가요?");
    const gameRound = await this.getInputCheckValidation(
      input,
      this.validateRound
    );
    this.#round = gameRound;
    return;
  }
  async getInputCheckValidation(inputInstance, validator) {
    while (true) {
      try {
        const inputValue = await inputInstance.getUserInput();
        const validateInputValue = validator(inputValue);
        return validateInputValue;
      } catch (e) {
        OutputValue.printOutput(e.message);
      }
    }
  }
  validateCars(userInput) {
    const splitName = userInput.split(",");
    if (!userInput) {
      throw new Error("[ERROR] 입력값이 없습니다.");
    }
    if (userInput.replace(",", "").trim().length === 0) {
      throw new Error("[ERROR] 자동차 이름은 공백일 수 없습니다.");
    }
    return splitName;
  }

  validateRound(round) {
    const roundNumber = Number(round);
    if (Number.isNaN(roundNumber)) {
      throw new Error("[ERROR] 숫자만 입력 가능합니다.");
    }
    if (roundNumber <= 0) {
      throw new Error("[ERROR] 0 이하의 숫자는 입력할 수 없습니다.");
    }
    return roundNumber;
  }

  async startGame() {
    for (let i = 0; i < this.#round; i++) {
      this.#cars.forEach((car) => {
        const random = Random.pickNumberInRange(0, 9);
        const canMove = this.canMove(random);
        car.moveForwardShowPositions(canMove);
      });
      OutputValue.printOutput("");
    }
    this.showWinnersName();
    return;
  }

  showWinnersName() {
    const farDistance = this.findMaxPositions();
    const winnersCars = this.#cars.filter((car) =>
      car.isSameDistance(farDistance)
    );
    const winners = this.nameOfWinners(winnersCars);

    OutputValue.printOutput(`최종 우승자: ${winners}`);
  }

  canMove(number) {
    if (number >= 4) {
      return true;
    }
  }
  nameOfWinners(cars) {
    let names = [];
    if (cars.length === 0) {
      return cars.getDriver();
    }
    for (let car of cars) {
      names.push(car.getDriver());
    }
    return names.join(", ");
  }

  findMaxPositions() {
    if (this.#cars.length === 0) {
      throw new Error("차량 리스트가 비었습니다.");
    }
    this.#cars.sort((a, b) => b.compareDifferenceDistance(a));
    return this.#cars.reduce((maxCarPosition, currentCarPosition) => {
      if (
        currentCarPosition.compareDifferenceDistance(currentCarPosition) > 0
      ) {
        return currentCarPosition;
      }
      return maxCarPosition;
    });
  }
}

```

## 2) 실행

간단하게 실행할 때는 racing Game을 import해서 메소드를 간단히 실행할 수 있도록 했다! 뭔가 캡슐화를 이전 코드와 달리 구현해낸 것 같아 뿌듯하다 😉

```jsx
class App {
  async run() {
    const game = new RacingGame();
    await game.setGameCars();
    await game.setGameRound();
    game.startGame();
  }
}

```

# III. 느낀점

OOP와 RSP에 대해 이것저것 찾아보고, 이해하기 위해 블로그에 예시로 나온 코드도 다시 바꿔보고 해봤다. 쉽진 않았지만 무척 뿌듯하고 완성하고 나니 재밌다! 꺄륵