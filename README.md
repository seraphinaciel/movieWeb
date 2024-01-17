# Framer Motion

React용 production-ready 모션 라이브러리 (오픈 소스)

https://www.framer.com/motion
https://github.com/framer/motion

```bash
npx create-react-app my-app --template typescript
npm i recoil styled-components @types/styled-components react-router-dom framer-motion react-query react-hook-form -S
```

[v6와 다른 점](https://velog.io/@soryeongk/ReactRouterDomV6)

##

간단하게 애니메이션 작업하려면?
props

```js
<Input animate={{ scaleX: searchOpen ? 1 : 0 }} />
```

애니메이션을 프로그램에서 시작시키고 싶으면

- 동시에 애니메이션을 실행시킬 때 유용

1. 문자 그대로 스타일을 적는 법

## useAnimation

훅을 사용하여 시작 및 중지 메서드가 있는 AnimationControls을 만들 수 있다.

input

```js
function Header() {
  const inputAnimation = useAnimation();
  const toggleSearch = () => {
    if (searchOpen) {
      // trigger the close animation
      inputAnimation.start({ scaleX: 0 });
    } else {
      // trigger the open animation
      inputAnimation.start({
        scaleX: 1,
      });
    }
    setSearchOpen((prev) => !prev);
  };
  return (
    <Input
      animate={inputAnimation}
      transition={{ type: "linear" }}
      placeholder="search to movies or tv shows"
    />
  );
}
```

navigation

```js
function Header() {
  const navAnimation = useAnimation();
  const { scrollY } = useScroll();
  useMotionValueEvent(scrollY, "change", (latest) => {
    if (latest > 80) {
      navAnimation.start({ backgroundColor: "rgba(0,0,0,1)" });
    } else {
      navAnimation.start({ backgroundColor: "rgba(0,0,0,0)" });
    }
    console.log(latest);
  });

  return(
    <Nav animate={navAnimation} initial={{ backgroundColor: "rgba(0,0,0,0)" }}><Nav>
  )
}
```

## useMotionValueEvent

framer motion에서 지원, useEffect 동일한 역할을 한다

```js
const { scrollY } = useScroll();

// useEffect 동일
useEffect(() => {
  scrollY.onChange(() => console.log(scrollY.get()));
}, [scrollY]);

// useMotionValueEvent 동일
useMotionValueEvent(scrollY, "change", (latest) => {
  console.log(latest);
});
```

2. 변수를 지정함

```js
const navVariants = {
  top: { backgroundColor: "rgba(0,0,0,0)" },
  scroll: { backgroundColor: "rgba(0,0,0,1)" },
};
function Header() {
  const navAnimation = useAnimation();
  const { scrollY } = useScroll();
  useMotionValueEvent(scrollY, "change", (latest) => {
    if (latest > 80) {
      navAnimation.start("scroll");
    } else {
      navAnimation.start("top");
    }
    console.log(latest);
  });
  return (
    <Nav
      variants={navVariants}
      animate={navAnimation}
      initial={"top"}
    ><Nav>
  )
}
```
