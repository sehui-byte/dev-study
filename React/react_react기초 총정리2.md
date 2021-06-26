

### props.children

react에선 특수한 `props`인 `children`을 사용하여 자식 엘리먼트를 그대로 전달할 수 있다.

```javascript
function GreetComponent(props){
  return <div>
    <h1>Hello {props.name}</h1>
    {props.children}
    </div>
}

function App(){
  return (
    <GreetComponent name="react" children="2222">
      <p>It's still works</p>
      </GreetComponent>
   );
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

![image-20210523222931077](https://raw.githubusercontent.com/sehui-byte/save-image-repo/image/img/image-20210523222931077.png)

코드 실행결과는 위와 같다. 

`GreetComponent`의 내부에 있는 것들(`<p> ...`)이 `GreetComponent`의 `children prop`으로 전달되어 전달된 element들이 최종적으로 출력된다.



### React onChange

- [codepen에서 보기](https://codepen.io/sehui-byte/pen/PopQGRx?editors=0011)

```javascript

class Test extends React.Component {
  
  onChange(e) {
     console.log(e.target.value);
  }
  
  render() {
    return (
       <input type="text" name="name" onChange={this.onChange} />
    );
  }
}

ReactDOM.render(
  <Test />,
  document.getElementById('root')
);
```



---



## 참고자료

- [유튜브 - props.children](https://youtu.be/Sq0FoUPxj_c)

- react 공식문서