

오늘은 `react`의 `context`에 대해 정리하려고 한다.



`context`는 마치 **전역변수**와 비슷한 개념이라고 볼 수 있다.

`context`는 `mobX`나 `redux`와 같이 3rd party lib를 사용하지 않고,`state`를 관리하는, react 자체에서 제공하는 api이다. `context`는 react 16.3부터 제공되었다.



react는 부모 자식 간에 `props`를 통해서 데이터를 부모에서 자식으로 전달하는데 만약 depth가 너무 깊을 경우, 일일이 `props`는 전달하는 것은 굉장히 번거로운 일이다. 그러므로 `context`를 사용할 경우 일일이 자식에게 `props`로 넘겨주지 않아도 전역에서 공유된다.



### Context api

- createContext(defaultValue)
- Provider
- Consumer



----

## 참고자료

- https://kyounghwan01.github.io/blog/React/react-context-api/#context-api%E1%84%85%E1%85%A1%E1%86%AB
- https://ko.reactjs.org/docs/context.html#dynamic-context

- [Redux vs React context](https://slee2540.tistory.com/59)