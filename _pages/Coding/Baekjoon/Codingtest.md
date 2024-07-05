---
title: "코테준비"
tags:
    - 코테 공부
    - 개발 공부


date: "2024-06-25 22:30:23 +0900"
---
```jsx
something = {
  name:'Alice',
  do: ()=>{
    return 'Hi Im Alice'
  }
}

something.height = 150;
console.log(something)

function makeUser(name,height){
  return{
    name:name,
    height:height
  };
}
//키값 찾으려면 단순히 in
let AUser = makeUser('yubeen',150)
if('name' in AUser){
  console.log('is in!');
}
//값을 찾으려면 Object.values로 배열 가져오고 includes로 확인하기 

if (Object.values(AUser).includes('yubeen')){
  console.log('is in Yubeen')
}

for (let value of Object.values(AUser)){
  console.log(value);
}
//AUser의 값은 보통 키값만 반환되는 듯, 키값이 정수이면 정렬되어서 반환됨.
for (let ject in AUser){
  console.log(ject)
}

//set이면 add이고, 값이 있는지 볼려면 has() delete, clear, size
//map.set('key','value'); get(key), has(key) delete(key),size
//object는 Object.함수(객체)로 보통 함수를 쓴다.
//Object.keys(obj) .values(obj) entries(obj)키,걊 쌍을 배열로
//arr.push, pop() unshift() 왼쪽에 요소 추가
//arr.concat([1,2,4]) ->arr배열에 124 추가
//slice(start,end) 
//arr.forEach(element=>console.log(element)); 각 요소에대해 함수 실행
//let new = arr.map(x=>x*2); 각 요소에 함수 실행 후! 새로운 배열  
//filtered = arr.filter(x=>x>2) 함수 중 참인 요소만여 배열로 반환
//arr.includes(4)->boolean반환. 
// for문 돌릴때는 for(let ~~ of arrya)









const board = [5, 20, 3, 4, 1, 9, 11, 13, 7, 9, 11, 13, 14, 15, 18, 19, 31, 22, 2, 6];
const boardSize = board.length;
function solution(scoreList) {
  const N = scoreList.length;
  if (N === 0 || N % 7 !== 0) return "ERROR";  // scoreList 길이가 0이거나 7의 배수가 아니면 "ERROR" 반환

  // N = scoreList의 길이, 만약 N이 0이거나 N이 7로 나누어 떨어지지 않으면 "ERROR"를 반환해요.

  for (let i = 0; i < N; i += 7) {
      if (scoreList[i] >= boardSize) return "BOARD";  // 범위 시작 인덱스가 보드 크기를 초과하는 경우 "BOARD"
  }

  // scoreList의 각 7개씩 끊어서, 첫 번째 숫자가 보드 크기 (boardSize)를 초과하는지 확인해요.
  // 만약 초과하면 "BOARD"를 반환해요.

  let a = 0;
  let b = 0;

  // A와 B의 점수를 저장할 변수 a와 b를 0으로 초기화해요.

  for (let i = 0; i < N / 7; i++) {
      const firstRangeIndex = scoreList[7 * i];
      const indexRange = new Set();

      for (let j = 0; j < 3; j++) {
          indexRange.add((firstRangeIndex + j) % boardSize);  // 범위 설정
      }

      // 첫 번째 숫자 (firstRangeIndex)로부터 3개의 연속된 인덱스를 계산해요.
      // 이 인덱스들은 보드 크기 (boardSize)를 초과하지 않도록 나머지 연산을 사용해요.

      const aRange = [];
      const bRange = [];

      for (let j = 0; j < 3; j++) {
          aRange[j] = scoreList[(7 * i) + 1 + j];
          bRange[j] = scoreList[(7 * i) + 4 + j];
      }

      // aRange와 bRange 배열에 각각 3개의 값을 넣어요.
      // scoreList의 7개 묶음 중 두 번째부터 네 번째 값은 aRange,
      // 다섯 번째부터 일곱 번째 값은 bRange로 설정해요.

      let bonusA = true;
      let bonusB = true;

      if (aRange[0] !== aRange[1] || aRange[1] !== aRange[2]) bonusA = false;
      if (bRange[0] !== bRange[1] || bRange[1] !== bRange[2]) bonusB = false;

      // aRange와 bRange의 세 값이 모두 같은지 확인해요.
      // 만약 하나라도 다르면 bonusA와 bonusB를 false로 설정해요.

      for (let j = 0; j < 3; j++) {
          if (indexRange.has(aRange[j])) {
              a += board[aRange[j]];
          } else {
              bonusA = false;
          }

          if (indexRange.has(bRange[j])) {
              b += board[bRange[j]];
          } else {
              bonusB = false;
          }
      }

      // aRange와 bRange의 값이 indexRange에 있는지 확인하고,
      // 있다면 a와 b의 점수를 보드 값만큼 더해요.
      // 만약 값이 없다면 해당 bonus를 false로 설정해요.

      if (bonusA) a += board[aRange[0]];
      if (bonusB) b += board[bRange[0]];
  }

  // 모든 범위가 같은 값을 가지고 있다면 보너스 점수를 추가해요.

  let res;
  if (a === b) res = "=";
  else if (a > b) res = ">";
  else res = "<";

  // A와 B의 점수를 비교해 결과 문자열을 결정해요.
  // 같은 점수면 "=", A가 더 크면 ">", B가 더 크면 "<"를 설정해요.

  return `${a}${res}${b}`;
}

// 최종적으로 A와 B의 점수를 비교한 결과와 함께 점수를 반환해요.
```