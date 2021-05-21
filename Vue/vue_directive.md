[[맨땅에 VueJS] 예제로 배우는 VueJS를 시작하며 / VueJS 강의](https://medium.com/@hozacho/%EB%A7%A8%EB%95%85%EC%97%90-vuejs-%EB%A6%AC%EC%8A%A4%ED%8A%B8-462d88047893)
정리한 내용

--> 거의 라우팅 위주의 강의

jsfiddle 사용 (HTML) (JavaScript)

jsfiddle : 따로 ide 쓰지않고 인터넷사이트에서 바로 코드 넣으면 결과나옴

- 기본적인 형식

Vue 생성자 함수를 이용하여 인스턴스를 생성하는 방법이다.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <p>{{ title }}</p>
  <p>{{ howAreYou() }}</p>
</div>
<!--출력 : 
안녕 Vue!
기분이 어때? -->
```

```jsx
new Vue({
  el: '#app', // el -> 어떤 요소에 적용할지 정함
  data: {  // 글자 출력
    title: '안녕 Vue!'
  },
  methods: { // 함수부분
  	howAreYou: function(){
  	return "기분이 어때?"
  	}
  }
})
```
→ 여기서는 el에서 div id가 "app"이었으니 "`#app"` class 였으면 `".app"`


  **디렉티브** : html 태그안에 들어가는 속성의 역할을 하며, `v-`라는 접두사가 붙는 것이 특징

### v-bind 디렉티브

html 속성 내에서 Vue에 선언된 값을 사용하고 싶은 경우

html속성에는 {{ mustached }} 사용 불가

`<a href="{{ link }}">링크</a>` → 이렇게 사용할 수 없음

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <a v-bind:href="link">링크</a>
</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
    link:"https://medium.com"
  },
  methods: {
  }
})
```

v-bind 디렉티브로 class와 style 사용하여 원하는 스타일링 가능

→ [4.v-bind:class 참고](https://www.notion.so/4-v-bind-class-v-bind-style-d29b59d26a19487eb58ec557a87ce585)

### v-once

Vue는 Vue instance 내의 데이터와 DOM이 연결되어 모든 것이 반응형으로 작동

→ function에서 title 값 변동시키면 title 값 자체가 변동됨

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h1>{{ title }}</h1>
<!-- <h1 v-once>{{ title }}</h1>-->
  <p>{{ sayHello() }}</p>
</div>
<!--v-once하기 전
안녕하십니까!
안녕하십니까!
v-once후 
안녕 VueJS!
안녕하십니까! -->
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
    title: "안녕 VueJS!",
  },
  methods: {
    sayHello: function () {
     this.title = "안녕하십니까!"
     return this.title
    }
  }
```

v-once 디렉티브는 HTML코드로 출력이 된 이후에 어떤 후처리가 있더라도 처음에 출력한 값을 유지시킬 때 사용

### v-html

html코드를 직접적으로 입력할 때 사용

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <p v-html="aLinkToMedium"></p>
</div>
```

```html
new Vue({ 
  el: "#app", 
  data: {
    aLinkToMedium: "<a href='http://medium.com'>링크</a>"
  }
})
```

- **v-bind 와 v-html**

**v-bind**

html 속성 내에서 Vue에 선언된 값을 사용

 html - `<a v-bind:href="link">링크</a>`

 javaScript - `data: {link:"https://medium.com"},`

### v-on

이벤트리스너

`v-on:이벤트이름=" method 이름"`

 v-on 이용하여 주사위 던지기 -> 맨 하단에 기술

클릭 : `v-on:click="함수"`

더블클릭 : `v-on:dblclick="함수"`  — ***db**click* 아님 ***db'l'**click*

마우스오버 : `v-on:mousemove="함수"`

그 이외 마우스 이벤트

[The MouseEvent](https://www.w3schools.com/jsref/obj_mouseevent.asp)

v-bind 약어

`v-bind:href="url"` → `:href="url"`

v-on 약어 

`v-on:click="doSomething"` → `@click="doSomething"`


- 키보드 이벤트 : `v-on:keyup.키값=”함수이름”`

    ```jsx
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <div id="app">
      <input 
        v-on:keyup.enter="login"
        placeholder="아이디를 입력하세요."
      >
    </div>
    ```

    ```jsx
    new Vue({ 
      el: "#app", 
      methods:{
        login() {
          alert('로그인 되었습니다.')
        }
      }
    })
    ```



# v-on 이용 주사위 던지기
## **1. 로직 생각해보기**

아래와 같은 로직으로 주사위 던지기를 만들어 보겠습니다.

1) 주사위 **수를 나타내는 값**을 하나 정해야합니다.

2) 주사위를 던졌을 때, **1–6사이의 랜덤한 숫자**를 반환해야합니다.

자 그럼, 우리가 이때까지 배운 것을 토대로 위의 말을 바꿔보겠습니다.

1)주사위 **수를 나타내는 값**을 하나 정해 `data`에 선언해 줘야합니다.

2–1) `methods` 내에 **1–6사이의 랜덤한 숫자**를 반환하여 `data`에 선언해준 값을 바꿔주는 함수를 생성해야 합니다.

2–2) 주사위를 던졌을 때 함수를 실행할 수 있도록 **이벤트 리스너인 `v-on`을 사용**해야 합니다.

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
{{number}} <!--주사위 수-->
<button v-on:click="rollDice">주사위던지기</button>
</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
     number: 0 
     },
   methods: {
   	rollDice: function(){
    let N = Math.floor(Math.random()*6)+1;
		// 인스턴스 내부의 값을 불러올 때는 this를 사용
    this.number = N
    }
   }
})

```

인자 정해서 넘기는 경우

```jsx
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <p>{{ count }}</p>
  <button v-on:click="addCount(2)">2를 더해줘</button>
</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
  	count: 0,
  },
  methods:{
    addCount(number) {
      this.count = this.count + number
			//this.count += number
    }
  }
})

// 버튼 누르면 2씩 더해짐
```