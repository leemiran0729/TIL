# useCallback (hook)

- 함수 자체를 저장하는 것을 의미
  - 메모이제이션된 콜백을 반환
- useCallback(function, deps)

```javascript
const value = useCallback(() => {}, []);
```

- useCallback의 예

```javascript
import {useEffect, useState} from "react";

function App() {
  const [number, setNumber] = useState(0);
  const [toggle, setToggle] = useState(true);

  /*const f = () => {
    console.log(${number});
  }*/

  //toggle이 바뀌어도 number가 바뀌지 않은 이상 리렌더링 방지됨
  const f = useCallback(() => {
    console.log(${number})
  }, [number]);

  // 함수도 객체이므로 number가 바뀌면 App이 리렌더링되어 useEffect가 실행됨
  useEffect(() => {
    console.log("f 함수가 변경되어 useEffect가 실행됨");
  }, [f]);

  return (
    ...
  )
}
```
