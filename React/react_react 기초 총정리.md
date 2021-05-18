### 5월 스터디 목표

- **front - React, TypeScript**
- **back - SpringBoot**

일단 1주차에 리액트 기초부분에 대한 개념을 다지고, 2주차에 spring boot를 공부하며, 3-4주차에 간단한 미니 프로젝트를 진행하는 식으로 스터디를 진행해보려고 한다.

----------



<목차>

- React란?
- JSX
- props, state
- React Component lifeCycle
- 이벤트 처리하기



---



### React란?

- **React.js** : **UI를 위한 javascript의 라이브러리.** (페이스북에서 만들었다.)

- **SPA(Single Page Application)**

- 라우팅, api 통신 등의 기능이 리액트에서 제공되지 않으므로 3rd party 라이브러리를 사용해야한다. → axios, mobX, redux 등

- **Babel** 을 통해 브라우저에서 실행되기 전에, 대부분의 브라우저가 사용할 수 있는 JS코드로 변환하여 사용된다.

- **Virtual DOM** : React는 DOM객체를 직접적으로 조작하지 않고, virtual DOM을 만들어서 실제 DOM과 virtual DOM의 차이점을 비교하고, 변경된 부분을 실제 DOM에 적용한다. 그렇기 때문에 **동적인 웹에서 리액트를 사용할 경우 더 나은 성능**을 기대할 수 있다.

- **컴포넌트**를 통해 UI를 재사용 가능한 여러 조각으로 나누며 독립적인 단위로 쪼개어 생각할 수 있다.

  - **컴포넌트 : React element를 반환.**

  - 컴포넌트의 이름은 항상 대문자로 시작한다.

  - 컴포넌트 합성 : 컴포넌트는 자신의 출력에 대해 다른 컴포넌트를 참조할 수 있다.

    ```react
    function Welcome(props){
        return <h1>hello, {props.name}</h1>;
    }
    ```

    ```react
    class Welcome extends React.Component{
        render(){
            return <h1>Hello, {this.props.name}</h1>
        }
    }
    ```

    

### JSX

- **JSX(JavaScript Extension)** : html을 이용할 때 JSX를 사용한다. 

  - JSX나 ES6 등의 경우 모든 브라우저에서 지원하지 않기 때문에 브라우저가 해석이 가능한 코드로 변환해야 한다. ex) `Babel`
  
  ```jsx
  //JSX
  const element = <h1 className="greeting">hello react</h1>;
  
  //Babel은 JSX를 React.createElement()호출로 컴파일한다.
React.creatElement('h1', {className: 'greeting'}, 'hello react');
  ```

  - 위 문법은 문자열도 아니고, html도 아니다. **JSX라고 하여 javascript를 확장한 문법이다. JSX는 react element를 생성**한다.  react를 사용할 때 JSX가 필수는 아니지만, 편리하기 때문에 주로 같이 사용하는 듯 하다. (일일이 `React.createElement()`하는건 귀찮으니까..)

  - `{}` 중괄호 안에 자바스크립트 표현식을 넣을 수 있다.

  - JSX는 표현식이다. 컴파일이 끝나면 JSX 표현식이 정규 javascript함수를 호출하고, javascript객체로 인식된다.
  
    ```jsx
    const name = 'ksh';
    const element = <h1>hello, {name}</h1>;
    
    ReactDOM.render(
    	element,
        document.getElementById('root')
  );
    ```

  - `()`로 묶어서 여러줄로 정의할 수도 있다.
  
    ```jsx
    const element = (
    	<div>
        	<h1>{name}</h1>
            <p>hello react</p>
        </div>
  );
    ```
  
    

### props, state

**리액트 컴포넌트는 두가지 property를 가진다. → props, state**

- 리액트 컴포넌트를 이해하기 위한 예제 : https://codepen.io/sehui-byte/pen/yLgmXEw

  (react공식문서를 보고 이해하기 위해 codepen에 따라 쳐보았다.)

  ```react
  function formatDate(date) {
    return date.toLocaleDateString();
  }
  
  //Comment 컴포넌트
  function Comment(props) {
    return (
      <div className="Comment">
        <div className="UserInfo">
          <img
            className="Avatar"
            src={props.author.avatarUrl}
            alt={props.author.name}
          />
          <div className="UserInfo-name">
            {props.author.name}
          </div>
        </div>
        <div className="Comment-text">{props.text}</div>
        <div className="Comment-date">
          {formatDate(props.date)}
        </div>
      </div>
    );
  }
  
  const comment = {
    date: new Date(),
    text: 'I hope you enjoy learning React!',
    author: {
      name: 'Hello Kitty',
      avatarUrl: 'https://placekitten.com/g/64/64',
    },
  };
  
  
  const comment2 = {
    date : new Date(),
    text: 'hello react!',
    author: {
      name : 'ksh',   avatarUrl:'https://placekitten.com/g/64/64'
    }
  };
  
  //화면에 렌더링
  ReactDOM.render(
   <React.Fragment>
    <Comment
      date={comment.date}
      text={comment.text}
      author={comment.author}
    />
     <Comment
      date={comment2.date}
      text={comment2.text}
      author={comment2.author}
    />
      </React.Fragment>,
    document.getElementById('root')
  );
  
  ```

  `props`와 `state` 두 객체 모두 렌더링 결과물에 영향을 주는 정보를 갖고 있는데 `props`는 마치 함수 매개변수와 같이 컴포넌트에 전달되고, `state`는 함수 내에 선언된 변수처럼 컴포넌트 안에서 관리된다.

  

- **`props`** 

  - **부모 컴포넌트가 자식 컴포넌트에게 값을 전달**할 때 사용하는 것.

  - **`read-only`** : <u>컴포넌트 자체(내부)에서 수정 불가능.</u> 

    - 그러므로 상황에 따라 변경되어야 하는 값들은 `state`를 이용한다.

    ![img](https://miro.medium.com/max/1708/1*1JzmOFt70B-EF3rQzrI9PQ.png)

    ```react
    function Welcome(props){
        return <h1>hello, {props.name}</h1>;
    }
    
    function App(){
        return(
        	<div>
            	<Welcome name="Sara"/>
                <Welcome name="Cahal"/>
                <Welcome name="Edite"/>
            </div>
        );
    }
    
    ReactDOM.render(
    	<App />,
        document.getElementById('root')
    );
    ```

- **state**

  - **컴포넌트 자신이 가지고 있는 값**. 그러므로 **컴포넌트 안에서 관리**된다.

  - `setState()`를 통해 수정할 수 있다. 

    - `setState`를 사용하지 않고 `state`의 값을 직접 변경할 경우 해당 오브젝트의 reference값이 변하지 않아 컴포넌트의 `state`는 변경되지 않고, 화면이 갱신되지 않는다. 
    - `setState`는 **비동기**로 동작한다. 끊김없는 원활한 UI/UX를 제공하기 위해 `render`를 꼭 수행시키기 위해서이다. 

    아래는 react공식문서에 나와있는 Clock 예제이다. clock의 경우 매순간마다 자체적으로 렌더링하여 시각을 바꿔야 한다. 그렇기 때문에 `props`가 아닌 `state`를 사용한다.

  ```react
  class Clock extends React.Component{
      //클래스 컴포넌트는 항상 props와 함께 기본생성자를 호출한다
      constructor(props){
          super(props);
          this.state = {date : new Date()};
      }
      
      render(){
          return(
              <>
                  <h1>clock example</h1>
                  <h2>{this.state.date.toLocalTimeString()}</h2>
              </>
          );
      }
  }
  
  ReactDOM.render(
  	<Clock />,
      document.getElementById('root')
  );
  ```

  그런데 위 코드는 컴포넌트 생성시에만 `state`값이 초기화되어 렌더링되고, 매순간 시간이 흐를 때마다 `state`값이 변경되면서 닷 렌더링되는 과정이 없다. 그러기 위해선 **컴포넌트 라이프사이클 개념**에 대해 이해해야 한다.

  

### React Component lifeCycle

![screenshot-from-2016-12-10-00-21-26](https://velopert.com/wp-content/uploads/2016/03/Screenshot-from-2016-12-10-00-21-26-1.png)

![React) Component Life Cycle Methods · Jistol Github Page](https://jistol.github.io/assets/img/frontend/react-lifecycle-methods/2.png)

- **mounting : 화면에 컴포넌트를 그린다**
- **updating : 컴포넌트를 갱신한다**
- **unmounting : 화면에서 컴포넌트를 지운다**



``` react
class Clock extends React.Component{
    //클래스 컴포넌트는 항상 props와 함께 기본생성자를 호출한다
    constructor(props){
        super(props);
        this.state = {date : new Date()};
        
        
        //lifecycle
        //componentDidMount() : 컴포넌트가 렌러딩된 후에 실행된다
        componentDidMount(){
            this.timerID = setInterval(
            	() => this.tick(),
                1000
            );
        }
        
        //
        tick(){
            this.setState({
                date: new Date()
            });
        }
    }
    
    render(){
        return(
            <>
                <h1>clock example</h1>
                <h2>{this.state.date.toLocalTimeString()}</h2>
            </>
        );
    }
}

ReactDOM.render(
	<Clock />,
    document.getElementById('root')
);
```



### 이벤트 처리하기

- React에서 이벤트는 `camelCase`를 사용한다. → `onClick`
- JSX를 사용하여 문자열이 아닌 함수로 이벤트 핸들러 전달.
  - `onclick={handleClick}`



```javascript
function ActionLink(){
  //e : 합성 이벤트(SyntheticEvent) : 모든 브라우저에서 동작
  function handleClick(e){
    e.preventDefault();
    console.log('the link was clicked');
  }
  
  return(
    <a href="#" onClick={handleClick}>
    Click me
    </a>
  );
}

ReactDOM.render(
	<ActionLink />,
    document.getElementById('root')
);
```



아래는 토글버튼 예제이다. 클릭할 때마다 `state`를 변화시킨다.

```javascript
class Toggle extends React.Component{
  constructor(props){
    super(props);
    this.state = {isToggleOn : true};
    
    //callback func에서 this가 작용하려면 아래와 같이 바인딩 필요
    this.handleClick = this.handleClick.bind(this);
  }
  
  handleClick(){
    this.setState(state => ({
      isToggleOn : !state.isToggleOn
    }));
  }
  
  render(){
    return(
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ?  'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

또는 `arrow function`을 이용하여 `this`바인딩을 할 수도 있다.

```react
<button onClick={() => this.handleClick()}>Click me</button>
```









---------------



### + Thinking in React

리액트의 기본 개념에 대해 이해하기 좋은 유명한 예제이다. 

![Component diagram](https://ko.reactjs.org/static/eb8bda25806a89ebdc838813bdfa3601/6b2ea/thinking-in-react-components.png)

- codepen : https://codepen.io/sehui-byte/pen/ZELowoP

```javascript
class ProductCategoryRow extends React.Component {
  render() {
    const category = this.props.category;
    return (
      <tr>
        <th colSpan="2">
          {category}
        </th>
      </tr>
    );
  }
}

class ProductRow extends React.Component {
  render() {
    const product = this.props.product;
    const name = product.stocked ?
      product.name :
      <span style={{color: 'red'}}>
        {product.name}
      </span>;

    return (
      <tr>
        <td>{name}</td>
        <td>{product.price}</td>
      </tr>
    );
  }
}

class ProductTable extends React.Component {
  render() {
    const rows = [];
    let lastCategory = null;
    
    this.props.products.forEach((product) => {
      if (product.category !== lastCategory) {
        rows.push(
          <ProductCategoryRow
            category={product.category}
            key={product.category} />
        );
      }
      rows.push(
        <ProductRow
          product={product}
          key={product.name} />
      );
      lastCategory = product.category;
    });

    return (
      <table>
        <thead>
          <tr>
            <th>Name</th>
            <th>Price</th>
          </tr>
        </thead>
        <tbody>{rows}</tbody>
      </table>
    );
  }
}

class SearchBar extends React.Component {
  render() {
    return (
      <form>
        <input type="text" placeholder="Search..." />
        <p>
          <input type="checkbox" />
          {' '}
          Only show products in stock
        </p>
      </form>
    );
  }
}

class FilterableProductTable extends React.Component {
  render() {
    return (
      <div>
        <SearchBar />
        <ProductTable products={this.props.products} />
      </div>
    );
  }
}


const PRODUCTS = [
  {category: 'Sporting Goods', price: '$49.99', stocked: true, name: 'Football'},
  {category: 'Sporting Goods', price: '$9.99', stocked: true, name: 'Baseball'},
  {category: 'Sporting Goods', price: '$29.99', stocked: false, name: 'Basketball'},
  {category: 'Electronics', price: '$99.99', stocked: true, name: 'iPod Touch'},
  {category: 'Electronics', price: '$399.99', stocked: false, name: 'iPhone 5'},
  {category: 'Electronics', price: '$199.99', stocked: true, name: 'Nexus 7'}
];
 
ReactDOM.render(
  <FilterableProductTable products={PRODUCTS} />,
  document.getElementById('container')
);

```





--------

### 참고자료

- 리액트 공식문서
- [나무위키](https://namu.wiki/w/React(%EB%9D%BC%EC%9D%B4%EB%B8%8C%EB%9F%AC%EB%A6%AC))

- [react의 기본 컴포넌트를 알아보자](https://medium.com/little-big-programming/react%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EC%BB%B4%ED%8F%AC%EB%84%8C%ED%8A%B8%EB%A5%BC-%EC%95%8C%EC%95%84%EB%B3%B4%EC%9E%90-92c923011818)

- 컴포넌트 라이프사이클 관련 게시물 : https://velopert.com/1130

- [thinking in react 번역 ](https://berkbach.com/react-docs-%EB%B2%88%EC%97%AD-thinking-in-react-8b8b1fb0c2e2)

