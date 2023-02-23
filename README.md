# movieWeb

### 23.2.23.

## React JS란?

javascript보다 간단한 코드로 ui를 인터랙티브하게 한다.

## react 설치

react, react-dom을 import한다.

```
<script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
<script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
```

`react js` : 어플리케이션이 인터랙티브 하도록 만들어 주는 라이브러리

`react-dom` : 라이브러리/패키지, 리액트 요소들을 html body에 둘 수 있도록 한다.

## UI를 그려보자!

_원본_

```javascript
<body>
  <span>total clicks : 0</span>
  <button id="btn">click me</button>
</body>

<script>
  let counter = 0;
  const button = document.getElementById("btn");
  const span = document.querySelector("span");
  function handleClick() {
    counter = counter + 1;
    span.innerText = `total clicks: ${counter}`;
  }
  button.addEventListener("click", handleClick);
</script>
```

### React 구조를 이해하기 위한 UI를 어려게 그려본다.

~~아무도 이렇게 사용하지 않는다. JSX를 사용하지~~

`ReactDOM.render()` : render의 의미는 react element를 가지고 HTML로 만들어 배치, 사용자에게 보여준다.

`ReactDOM.render(span, span이 가야할 위치)`: body안에 #root 만들어서 사용

`React.createElement("span", {span의 property}, “span의 내용”)`

```javascript
const root = document.getElementById("root");
const Title = React.createElement(
  "h3",
  { id: "title", style: { color: "tomato" } },
  "Hello I'm a Title"
);
const btn = React.createElement(
  "button",
  {
    style: { backgroundColor: "tomato" },
    onClick: () => console.log("Im clicked"),
  },
  "Click me"
);
const container = React.createElement("div", null, [Title, btn]);
ReactDOM.render(container, root);
```

### JSX로 편하게 UI를 그린다.

`JSX` : javascript를 확장한 문법, html과 유사하게 리액트 요소를 만들게 해준다.
`babel`(변환기)로 변환을 시켜야 함

```
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
<script type="text/babel"> 내용 </script>
```

label의 for, class -> javascript가 선점하고 있으니 className, htmlFor로 써줘야 한다.

#### 1. component

`JSX` 는 컴포넌트를 따로 만들어서 합치면 되는 구조이다.

내가 만든 요소(component)의 첫 글자는 **대문자**로 시작해야 한다. 소문자는 html 태그가 됨.

`component` : 함수기 때문에 재사용이 가능하다.

```javascript
const root = document.getElementById("root");
const Title = () => (
  <h3
    id="title"
    style={{ color: "tomato" }}
    onMouseEnter={() => console.log("mouse enter")}
  >
    Hello I'm a Title
  </h3>
);
const Btn = () => (
  <button
    style={{ backgroundColor: "tomato" }}
    onClick={() => console.log("clicked")}
  >
    Click me
  </button>
);
const Container = () => (
  <div>
    <Title />
    <Btn />
  </div>
);
ReactDOM.render(<Container />, root);
```

> 아래의 두 문장은 함수를 만들어 return하는 것과 같다.

> ```
> const Title = () => ( content )
> function Title() { return( content ); }
> ```

#### 2. counter 만들기

1. ui 업데이트 하려면 랜더 함수 다시 부르자

리액트 장점 : **ul에서 바뀌는 부분만 업데이트 해줌**

하지만 아래는 버튼을 누를 때마다 `render()`을 직접 불러 리랜더링 된다.(수동)

```javascript
const root = document.getElementById("root");
let counter = 0;
function countUp() {
  counter = counter + 1;
  render();
}
function render() {
  ReactDOM.render(<Container />, root);
}
const Container = () => (
  <div>
    <h3>Total clicks: {counter}</h3>
    <button onClick={countUp}>Click me</button>
  </div>
);
render();
```

---

2. 자동으로 리랜더링 시키자

**첫번째 방법 : 직접 값을 설정해준다.**

`React.useState(0)` : `countUp()`와 동일한 역할을 한다.

useState는 배열을 제공한다.

`counter` : 첫번째 요소, 초기값, 현재값

`setCounter` : 두번째 요소, 현재값을 바꾸는 함수, 첫번째 값을 변경하고 컴포넌테를 새로고침 한다.
`setCounter`를 통해 새로운 값을 가지고 컴포넌트 전체가 재생성 되며 ui의 일부(리액트 장점)가 업데이트 된다.
하지만 계산을 하다가 counter가 다른 곳에서 변경되어서 생각했던 값이 아니면...결과가 달라지게 된다.

```javascript
const root = document.getElementById("root");
function App() {
  const [counter, setCounter] = React.useState(0);
  const onClick = () => {
    setCounter(counter + 1);
  };
  return (
    <div>
      <h3>Total clicks: {counter}</h3>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}
ReactDOM.render(<App />, root);
```

**두번째 방법 : 함수로 전달하기**

다음 state의 값이 현재 값을 바탕으로 계산이 되도록 함수를 이용한다.
그러면 언제나 현재 state의 값을 얻도록 해준다.

```javascript
const root = document.getElementById("root");
function App() {
  const [counter, setCounter] = React.useState(0);
  const onClick = () => {
    setCounter((current) => current + 1);
  };
  return (
    <div>
      <h3>Total clicks: {counter}</h3>
      <button onClick={onClick}>Click me</button>
    </div>
  );
}
ReactDOM.render(<App />, root);
```

<!-- -

-
-

const array = [0,1,2,3]
const [a, b, c, d] = array -->
