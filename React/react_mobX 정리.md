### MobX

`mobX`는 react 상태 관리 라이브러리로 자주 사용되는 라이브러리이다. 



#### Observable

- `@observable`
- 관찰대상 State
- 관찰대상인 해당 state 값이 변경될 때마다 re-rendering 된다.



#### Observer

- `@observer`
- observable state가 업데이트되는 것을 감지한다. 



#### Inject

- `@inject`
- store를 props에 넣어주는 역할을 한다.

​	

#### Store

- 여러 스토어끼리 관계를 형성하기 위해 하나의 공통 스토어로 관리해야 한다.



----

### Store Injecting

> Store injection 패턴은 `mobx-react`라이브러리로 잘 알려진 패턴이다. 이는 지금은 component tree를 통해 MobX stores에 접근하는, 한물 간 방식이다. React legacy context가 사용하기 더 힘들었기 때문에 등장했었던 것이다.
>
> 이젠 2019년이고 우리는 더이상 injections이 필요하지 않다.
>
>  
>
> ### What is it obsolete?
>
> 주된 이유는 지금 우리에겐 더 나은 도구들이 있기 때문이다. inject pattern은 잘못되거나 망가진 것은 아니지만 제한적이고 오류가 발생하기 쉬운 패턴이다.  
>
> 매우 간단한 형식인 `inject('myStore')`에서 감싸여진 component props가 수정되면서 component에 무엇이 올지를 놓치기 쉽다. 왜냐하면 stirng args는 어떠한 단서도 제공하지 못하기 때문이다. 그래서 이게 성가신 버그를 유발하는 소스코드가 될 수도 있고, 특히 이 패턴은 <u>typescript와 같이 타입이 있는 언어와 호환되지 않는다.</u> 
>
> 대안이 되는 inject의 변형은 store들에 대한 반응적인 선택이 컴포넌트가 요구하는 값을 선택할 수 있도록 한다. 다수의 컴포넌트들이 재사용할 때 유용하지만, inject의 심플한 형태로 유사한 혼란을 가져올 수 있다.
>
> ```javascript
> const NameDisplayer = ({ name }) => <h1>{name}</h1>
> 
> const UserNameDisplayer = inject(stores => ({
>   name: stores.userStore.name,
> }))(NameDisplayer)
> 
> const App = () => <UserNameDisplayer name="Is this name used? Who knows." />
> ```



`mobx-react`에서는 이제 `Provider`와 `inject`대신 `React context`로 대체하여 사용하는 것을 권장한다.



#### Customizing inject

>Instead of passing a list of store names, it is also possible to create a custom mapper function and pass it to inject. The mapper function receives all stores as argument, the properties with which the components are invoked and the context, and should produce a new set of properties, that are mapped into the original:
>
>```javascript
>mapperFunction: (allStores, props, context) => additionalProps
>```

`@inject('StoreName')` 대신 **custom mapper function**을 만들어서 store를 inject할 수 있게 되었다. 

```javascript
const NameDisplayer = ({ name }) => <h1>{name}</h1>;

//userStore를 NameDisplayer 컴포넌트에 inject한다
const UserNameDisplayer = inject(stores => ({
    name: stores.userStore.name
}))(NameDisplayer);

const user = mobx.observable({
    name: "Noa",
});

const App = () => (
	<Provider userStore={user}>
    	<UserNameDisplayer />
    </Provider>
);

ReactDOM.render(<App />, document.body);
```





-----

### 참고자료

- https://woowabros.github.io/experience/2019/01/02/kimcj-react-mobx.html

- https://mobx-react.js.org/recipes-inject

- https://mobx.js.org/react-integration.html

- https://github.com/mobxjs/mobx-react