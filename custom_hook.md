# Custom Hook

- useState, useRef 등과 같이 직접 개발자가 hook을 만들어 냄 -> 반복되는 메소드를 하나로 줄여줌 -> 재사용성
- use로 시작해야함
- 커스텀 훅은 컴포넌트 구현이 비슷하지만 JSX가 아닌 배열 혹은 object를 return

- useInput

  ```javascript
  import { useState } from "react";

  function App() {
    const [value, setValue] = useState("");

    const handleChange = (e) => {
      setValue(e.target.value);
    };

    const handleSubmit = () => {
      alert(inputValue);
    };

    return (
      <div>
        <input value={value} onChange={handleChange}>
        ...
      </div>
    )
  }
  ```

  ```javascript
  //useInput.js
  import { useState } from "react";

  export function useInput(initialValue) {
    const [value, setValue] = useState(initailValue);

    const handleChange = (e) => {
      setValue(e.target.value);
    };

    return [value, handleChange];
  }

  //App.js
  import { useInput } from "./useInput";

  function App() {
    const [value, handleChange] = useInput("안녕");

    return (
      <div>
        <input value={value} onChange={handleChange}>
        ...
      </div>
    )
  }
  ```
