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

### React 구조를 이해하기 위한 UI를 어렵게 그려본다.

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

## converter 만들기

```javascript
function MinitesToHours() {
  const [amount, setAmount] = React.useState(0);
  const [inverted, setInverted] = React.useState(false);
  const onChange = (event) => {
    setAmount(event.target.value);
  };
  const reset = () => setAmount(0);
  const onInverted = () => {
    reset();
    setInverted((current) => !current);
  };
  return (
    <div>
      <div>
        <label htmlFor="minutes">Minutes</label>
        <input
          value={inverted ? amount * 60 : amount}
          id="minutes"
          placehoder="Minutes"
          type="number"
          onChange={onChange}
          disabled={inverted}
        />
      </div>
      <div>
        <label htmlFor="hours">Hours</label>
        <input
          value={inverted ? amount : Math.round(amount / 60)}
          id="hours"
          placehoder="Hours"
          type="number"
          onChange={onChange}
          disabled={!inverted}
        />
        <button onClick={reset}>reset</button>
        <button onClick={onInverted}>
          {inverted ? "Turn back" : "Invert"}
        </button>
      </div>
    </div>
  );
}

function KmToMiles() {
  const [amount, setAmount] = React.useState(0);
  const [inverted, setInverted] = React.useState(false);
  const onChange = (event) => {
    setAmount(event.target.value);
  };
  const reset = () => setAmount(0);
  const onInverted = () => {
    reset();
    setInverted((current) => !current);
  };
  return (
    <div>
      <div>
        <label htmlFor="km">KM</label>
        <input
          value={inverted ? Math.round(amount / 1000) : amount}
          id="km"
          placehoder="KM"
          type="number"
          onChange={onChange}
          disabled={inverted}
        />
      </div>
      <div>
        <label htmlFor="meter">Meter</label>
        <input
          value={inverted ? amount : amount * 1000}
          id="meter"
          placehoder="Meter"
          type="number"
          onChange={onChange}
          disabled={!inverted}
        />
        <button onClick={reset}>reset</button>
        <button onClick={onInverted}>
          {inverted ? "Turn back" : "Invert"}
        </button>
      </div>
    </div>
  );
}

function App() {
  const [index, setIndex] = React.useState("xx");
  const onSelect = (event) => {
    setIndex(event.target.value);
  };
  return (
    <div>
      <h1 className="hi">Super Converter</h1>
      <select value={index} onChange={onSelect}>
        <option value="xx">Select your units</option>
        <option value="0">Minutes & Hours</option>
        <option value="1">Km & Miles</option>
      </select>
      <hr />
      {index === "xx" ? "Please select your units" : null}
      {index === "0" ? <MinitesToHours /> : null}
      {index === "1" ? <KmToMiles /> : null}
    </div>
  );
}

const root = document.getElementById("root");
ReactDOM.render(<App />, root);
```

## component 재사용

`component` : 어떤 jsx를 반환하는 함수로 재사용이 가능하다.

### Property : props(인자)

> 부모 컴포넌트에서 자식 컴포넌트에 데이터를 보낼 수 있게 해주는 방식(통로)
> 컴포넌트 설정을 할 수 있다
> 자동으로 모든 property를 모조리 오브젝트 안에 넣는다. 이 오브젝트는 컴포넌트의 첫번째 인자(두번째 인자는 없음)로 주어진다.

**문법**

```javascript
function Btn(props) {
  {
    props.text;
  }
}

<Btn text="Save Changes" x={false} />;
```

```javascript
// 0
function Btn({ text, big, changeValue }) {
  return (
    <button
      onClick={changeValue}
      style={{
        backgroundColor: "tomato",
        color: "white",
        padding: "10px 20px",
        borderRadius: 10,
        border: 0,
        fontSize: big ? 18 : 16,
      }}
    >
      {text}
    </button>
  );
}

<div>
  // 1
  <Btn text={value} big={true} changeValue={changeValue} />
  // 2
  <Btn text="Continued" big={false} />
</div>;
```

- 0을 꼭 써줘야만 함, `fontSize = 14`로 초기화도 가능하다.
- 함수도 `props`이 될 수 있다. `changeValue`은 `addEventListener`가 아니고 `props`이다!

#### React Memo

부모 컴포넌트에 어떤 `props` 상태 변경이라도 있으면 자식들은 다시 그려진다(앱이 느려짐)
상태 변경이 없는 자식은 다시 그리지 말라고 해야함

위의 `1`의 props는 state와 연결되어 있으므로 다시 그리라고 요청(두 번 랜더링)
위의 `2`의 props는 다시 그리지 말라고 요청 (초기만 랜더링)

아래와 같이 `React.memo()`를 사용하면 필요 없는 상태변경은 하지 않아도 된다.

```javascript
const MemorizedBtn = React.memo(Btn);

<div>
  <MemorizedBtn text={value} changeValue={changeValue} />
  <MemorizedBtn text="Continued" />
</div>;
```

#### props type

어떤 타입의 props를 받고 있는지 체크

**설치**

```javascript
<script src="https://unpkg.com/prop-types@15.7.2/prop-types.js"></script>;

Btn.propTypes = {
  text: PropTypes.string.isRequired,
  fontSize: PropTypes.number,
};
```

> `text`는 문자열이어야 하며 필수다
> `fontSize`는 숫자여야 한다

오류나면 production을 development로 바꿔야 한다.

```
<script
  crossorigin
  src="https://unpkg.com/react@17.0.2/umd/react.development.js"
></script>
```

// const array = [0,1,2,3]
// const [a, b, c, d] = array
