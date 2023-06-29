# React.Memo

- 컴포넌트를 감싸서 사용하며 컴포넌트를 캐싱함
- 특히, 부모 컴포넌트 리렌더링시 자식 컴포넌트의 불필요한 리렌더링을 막아줌
  ```javascript
  function ParentComponent(props) {
    //변화...
    return (
      <>
        //부모 컴포넌트 리렌더링 시 자식 컴포넌트가 변화가 없어도 무조건
        리렌더링 됨
        <ChildComponent />
      </>
    );
  }
  ```
- React.memo는 오직 props 변화에마 의존하는 최적화 방법!
- React.memo 예

  ```javascript
  import { useState } from "react";
  import Child from "./Child";

  function App() {
    const [parentAge, setAge] = useState(0);
    const [childAge, setChildAge] = useState(0);

    const increaseParentAge = () => {
      setParentAge(parentAge + 1);
    }

    const increaseChildAge = () => {
      setChildAge(childAge + 1);
    }

    return (
      ...
      //ParentAge가 바뀌면 Child 컴포넌트도 리렌더링되는 현상 발생
      // 만약 Child 컴포넌트가 복잡한 로직이 있었다면...?
      <Child name={'이미란'} age={childAge}/>
    )
  }

  import {memo} from "react"

  function Child = ({name, age}) {
    ...
  }

  export default memo(Child);


  ```
