# useMemo (hook)

- 배경지식

  - 메모이제이션(Memoization): 이전 값을 메모리에 저장하여 동일한 계산 반복을 제거해 빠른 처리를 가능하게 함
    - 예를 들면 처음 본 문제의 답을 기억해놓고, 다시 또 그 문제를 보게되면 계산을 다시 할 필요없이 기억한 답을 체크하는 것과 같음

- useMemo: 값을 캐싱!
  - useMemo(function, deps)
    ```javascript
    const value = useMemo(() => {
      return ...; // 메모이제이션된 값을 반환
    }, []);
    /*
    1.[]에 값이 있을 경우
      1) 변화 O: 등록한 함수 호출하여 연산
      2) 변화 X: 이전 저장한 값을 재사용
    2. []에 값이 없을 경우
      : 처음 컴포넌트 마운트될 때만 값 계산
    */
    ```
- useMemo의 예시

  - useMemo 사용 안 한 경우: easyCalculate만 실행해도 delay 발생 -> setEasyNumber로 App이 리렌더링되면서 hardSum이 실행되기 때문
  - useMemo로 hardNumber만 변경할 때만 실행되게해서 렌더링 방지

    ```javascript
    import React, { useMemo, useState } from "react";

    const hardCalculate = (number) => {
      for (let i = 0; i < 99999999; i++) {
        return number + 100000 + i;
      }
    };

    const easyCalculate = (number) => {
      return number + 1;
    };

    function App() {
      const [hardNumber, setHardNumber] = useState(1);
      const [easyNumber, setEasyNumber] = useState(1);

      const hardSum = useMemo(() => {
        return hardCalculate(hardNumber);
      }, [hardNumber]);

      const easySum = easyCalculate(easyNumber);

      return
      (<>

      <div>
        <input type="number" value={hardNumber} onChange={(e) => setHardNumber(parseInt(e.target.value))}>
        <span>{hardSum}</span>
      </div>
      <div>
        <input type="number" value={easyNumber} onChange={(e) => setEasyNumber(parseInt(e.target.value))}>
        <span>{easySum}</span>
      </div>
      </>);
    }
    ```

  - 얕은 비교를 통해 object와 같은 참조타입이 deps로 비교하면서 useEffect가 계속 실행됨
  - 이를 방지하기 위해 참조타입의 location 값을 isKorea가 바뀔 때마다만 값을 변경하게끔 useMemo로 방지

    ```javascript
    import React, { useEffect, useMemo, useState } from "react";

    function App() {
      const [number, setNumber] = useState(0);
      const [isKorea, setIsKorea] = useState(true);

      /* 
      1. number가 바뀌어도 useEffect 실행안됨
      const location = isKorea ? "국내" : "국외";
      2. number가 바뀌면 useEffect 실행되어버림
      const location = {
        country: isKorea ? "국내" : "국외",
      };
      3. number가 바뀌어도 useEffect 실행안됨
      */
      const location = useMemo(() => {
        return {
          country: isKorea ? "국내" : "국외",
        };
      });

      useEffect(() => {
        console.log("useEffect 호출");
      }, [location]);
    }
    ```

- reference
  - https://jaylee-log.tistory.com/53
  - https://www.youtube.com/watch?v=e-CnI8Q5RY4
