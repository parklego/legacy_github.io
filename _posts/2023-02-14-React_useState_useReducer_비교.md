---
layout: post
title: 리액트 useState와 useReducer는 언제써야할까?
subtitle: 상태관리 Hooks 비교 + Context API
categories: React
tags: [React, useState, useReducer, useContext]
---

> 현재 라섹 수술 이후, 정상적인 시력으로 회복 중에 있다. 그래서 쉬면서 Udemy 강의를 통해 React 18버전으로 강의를 천천히 보고 있다.
>
> 실무에서는 대부분 useState를 써왔고, useReducer는 거의 사용하지 않았었는데 이번 기회를 통해 좀 더 깊게 탐구를 해보고 싶어졌고, 정리도 해보고 싶어졌다. 이번 포스팅으로 상태관리에 대한 부분은 헤매지 않았으면 하는 바람이다. 어떻게 import하고, state가 뭐고, 예제로 어떻게 사용하는지.. 등 기본적인 내용은 넘어간다.

 <br/>

## 1. useState

리액트에서 상태관리라고 하면, 가장 먼저 떠오르는 hooks 중 하나로 useState를 뽑을 수 있다.

컴포넌트에서 state를 동적으로 관리하기 위해서 자주 사용되는 함수이다.

```js
// 사용방법
const [state, setState] = useState(initialState);
```

```js
// 예시
const [name, setName] = useState("");

// 초기값
console.log(name); // ''

// name state 재할당
setName("에이프");

// 랜더링 이후
console.log(name); // 에이프
```

setName함수를 통해 name의 값을 변경해줄 수 있다. setName은 상태 값을 갱신해주는 Setter 함수라고 한다.

<br/>

## 2. useReducer

그렇다면, useReducer는 무엇인가? 앞서 사용법을 간단하게 정리해보자.

```js
// 사용방법
const [state, dispatch] = useReducer(reducer, initialState);
```

?? 사실 디스패치, 액션, 리듀서 바로 이해하긴 어렵다. 나 또한 한번에 이해하긴 힘들었으니까.

<br/>

### reducer

가장 중요할 것 같은 리듀서.

Reducer(리듀서)는 기본적으로 두 개의 인수(current state와 action)를 받고, 이렇게 받은 두 인수를 기반으로 해서 새 상태를 반환하는 함수이다.

### action

오케이. 리듀서는 이해했어. 근데 action은 뭐야?

action은 상태에 어떠한 변화가 필요하게 될 때, 발생시키는 것을 말한다. 액션 객체는 일반적으로 다음과 같은 형식으로 이뤄져있다.

```js
// 일반적으로 대문자와 _(언더바)를 조합하여 쓰는걸로 약속되어 있음.
{
  type: "INCREASE_NUMBER";
}
```

리듀서는 왜 사용하는건데? 3가지 이유를 들 수 있다.

1. 예측 가능하고 일관되게 작동하며, 그래서 복잡한 상태를 관리하는 데 적합하다.
2. 일반적으로 테스트하기 쉽다.
3. 실행 취소/다시 실행, 상태 지속성 등과 같은 헬퍼 함수는 reducer(리듀셔)로 구현하기가 더 쉽다.

state의 값을 특정 액션으로만 변경이 가능한 것이고, 특정 액션일 때 액션의 리듀서 변경 함수 대로 처리된다. 그래서 위와 같은 이점을 가지는 것이다.

이것이 리덕스에도 비슷한 개념으로 사용되는데 리덕스를 보면 역시나 예측가능하고 일관되게 작동하기 때문에 디버깅이 쉽다는 장점을 말하고 있다.

### dispatch

디스패치는 정해놓은 action과 데이터(선택사항)를 리듀서에 보내는 함수라고 생각하면 된다.

**useReducer를 이해하기 쉽게 정리하자면, dispatch를 통해 action을 보내면, reducer가 이를 받아 코드에 따라 적절하게 값을 변경한다고 정리하면 될 것 같다.**

## 3. useState vs useReducer

두 hooks 중에서 어떠한 경우에 어떤 함수를 사용 해야하는건지를 찾아봤다. 대부분의 블로그에서 일반적인 경우에는 useState, 복잡한 경우에는 useReducer를 쓰라고 한다.

아니! 복잡한게 어떤게 복잡한거냐고...!!! 너무 주관적이잖아..

아래와 같은 경우에 useReducer를 고려해보고, 그외에는 useState를 그대로 사용하면 될 듯 하다!

<br/>

#### 첫번째, 토글의 경우 사용 가능하다.

기존의 경우에는 토글부분을 구현할때, useState를 사용하여 콜백으로 사용하였다.

```js
const [toggle, setToggle] = useState(false);

<button onClick={setToggle((prev) => !prev)}>Toggle</button>;
```

<br/>
하지만, useReducer를 사용하는 것이 UI구현부에서 로직이 숨겨져 가독성이 더 좋은 것 같다.

```js
const [toggleState, dispatchToggleAction] = React.useReducer(prev => !prev, false)

<button onClick={dispatchToggleAction}>Toggle</button>
```

<br/>

#### 두번째, 복잡한 경우..?

관리할 상태가 여러개이고, 서로가 의존하고 있으며, 상태를 조작할 동작이 여러개일 때는 useReducer를 사용하는 것이 낫다.

여기에 해당하는 부분이 대부분의 블로그에서 말하는 복잡할 때에 해당하는 부분인 것 같다.

코드를 깔끔하게 하기위해서도 여기에 해당한다.

- 상태가 변하는 로직을 reducer 한 군데 몰아넣음으로써 상태 간의 관계를 한 번에 파악하기 용이하고, reducer에 로직을 몰아넣음으로써 훅을 깔끔하게 유지할 수 있다.
- 액션이 여러 개일 때 위 이유와 마찬가지로 리듀서 한 군데에서 액션을 관리할 수 있다.

<br/>

#### 세번째, 서버와 클라이언트 state를 분리 시켜야 할 때 사용한다.

reducer의 또 다른 좋은 특성은 인라인 하거나 클로져를 통해 props에 접근할 수 있다는 것입니다. 이 방법은 reducer 내부에서 props 또는 서버 state(예. useQuery hook이 리턴하는 state)에 접근할 때 매우 유용합니다. state 초기화를 통해 이러한 값들을 "복사"하지 않고도 reducer 함수에 전달할 수 있습니다.

```js
const reducer = (data) => (state, action) => {
  // ✅ 항상 최신 서버 state에 접근할 수 있습니다.
};

function App() {
  const { data } = useQuery(key, queryFn);
  const [state, dispatch] = React.useReducer(reducer(data));
}
```

<br/>

## 4. 정리

- 만약 state 변경이 독릭접으로 이뤄진다면 - 여러 useState로 분리하세요.

- 동시에 변경되거나 한 번에 하나의 필드만 변경(ex. 폼) 된다면 - 하나의 useState와 객체를 사용하세요.

- 사용자의 인터렉션이 state의 다른 부분을 변경한다면 (state가 서로 의존한다면) - useReducer를 사용하세요.

- 디버깅하기 용이하며, 로직을 분리하여 코드를 재사용하고, 깔끔하게 사용하고 싶다면 - useReducer를 사용하세요.

<br/>
<br/>

---

## 참고문서

[https://cpro95.tistory.com/642](https://cpro95.tistory.com/642)

[https://www.philly.im/blog/use-state-vs-use-reducer](https://www.philly.im/blog/use-state-vs-use-reducer)
