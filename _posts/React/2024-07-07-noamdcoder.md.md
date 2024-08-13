---
layout: post
title: PRACTICE MOVIE APP
subtitle: 리액트 공부하기
excerpt_image: https://nomadcoders.co/_next/image?url=https%3A%2F%2Fd1telmomo28umc.cloudfront.net%2Fmedia%2Fpublic%2Fthumbnails%2Freact-for-beginners.jpeg&w=750&q=75
categories: React
tags: [React,Frontend,노마드코더]

---

### useState(상태 초기값);


- 기능: 함수형 컴포넌트에서 상태를 관리함.
- return : 상태 변수, 상태 업데이트하는 함수 반환.
    - 상태 변수: useState(초기값);으로 초기화할 수 있다.
    - 상태 업데이트 함수: 새로운 상태 값을 받아서 상태를 업데이트 한다. 만약 업데이트 된다면, 컴포넌트를 다시 렌더링하여 상태를 반영한다.
- 예시

```jsx
const [coins, setCoins] = useState([]);
```

### useEffect(func,[업데이트 여부를 결정할 값])

---

- 기능:  코드를 넣으면 컴포넌트가 마운트 된 직후!+업데이트 될 때마다 실행
- 변수 2개 : (func, [업데이트 추적 값])
    - 첫번째는 실행할 함수를 넣는다. (마운트 직후+업데이트)
    - 두번째 인자는 업데이트 여부를 결정하므로 만약 `[] 빈 배열`인 경우, 처음 마운트 될 때만 실행된다.
    값이 있으면 `ex) [money]`  해당 값이 변경될 때 `func`가 실행된다.
- 예시

```jsx
useEffect(()=>{
    fetch('https://api.coinpaprika.com/v1/tickers')
    .then((response)=>response.json())
    .then((json)=>{
      setCoins(json);
      setLoading(false);
    });
  },[])
```

# CoinTracker

---

코인의 정보를 가져와서 보여줌.

### 추가 기능

---

사용자가 input으로 자신이 가진 돈을 넣으면(달러), 그

```jsx
import { useEffect, useState } from "react";
function App() {
  const [loading, setLoading] = useState(true);
  const [coins, setCoins] = useState([]);
  const [money, setMoney] = useState(0);
  useEffect(()=>{
    fetch('https://api.coinpaprika.com/v1/tickers')
    .then((response)=>response.json())
    .then((json)=>{
      setCoins(json);
      setLoading(false);
    });
  },[])
const onSubmit = (event)=>{
  event.preventDefault();

  if(money===""){
    return;
  }
    setMoney(event.target.value);

  console.log(money);
}
const onChange=(event)=>{
  console.log(event.target.value)
  setMoney(event.target.value);
}
  
  return (<div>
    <h1>The Coins!{coins.length}</h1>
    {loading ? <strong>Loading.. </strong> : null}

      <label>Enter Your money &nbsp;
      $<input name="money" onChange={onChange}></input>
      </label>

    <h2>You can buy </h2>
    <ol >
      {coins.map((coin,index)=>(
        <li key={coin.id} >
        {coin.name} : {coin.symbol} ${(coin.quotes.USD.price).toFixed(3)}
        <br>
        </br>You can buy this : {(money/coin.quotes.USD.price).toFixed(2)} coins
        
      </li>))}
      
    </ol>
  </div>
  );
```

## 정리 및 느낀점

---

- @tailwind components으로 state를 관리할 때, setCoins를 따로 기능을 구현해야하나 했지만 이미 상태 관리하라고 나온 함수이다. 오랜만에 해서 헷갈렸다.
- state를 항상 직접 수정하지 않는다. 수정하는 함수를 이용해서 수정한다!
- `…`는 스프레드 연산자이다. Array의 요소를 펼쳐주는 것! Python에서는 *가 …의 역할을 한다.
- map()에서 첫번째는 map이 돌아갈때 각 array요소이며,2번째는 index이다.

<aside>
💡 새로운 아이템을 만들 때 key가 필요한 이유:React가 업데이트를 효율적으로 관리하기 위해. 가상 DOM업데이트 방식이므로 key가 있어야 뭐가 변경,추가,삭제 되었는지 정확히 추적하기 때문이다. 일반적으로는 고유한 값 즉 데이터베이스 ID로 사용하는게 좋다.

</aside>

- useEffect는 컴포넌트가 시작될때 실행할 것을 넣으면 됨. 만약 ,  useEffect(()⇒{},[])에서 뒤에 값이 빈 배열이면, 한번만 실행되고  뒤에 값이 있으면 그게 변화되면 재실행해준다.
