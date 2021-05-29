[[맨땅에 VueJS] 예제로 배우는 VueJS를 시작하며 / VueJS 강의](https://medium.com/@hozacho/%EB%A7%A8%EB%95%85%EC%97%90-vuejs-%EB%A6%AC%EC%8A%A4%ED%8A%B8-462d88047893)
정리한 내용

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


## v-model

양방향 데이터 바인딩 

지금까지 앞에서 한 것은 단방향 데이터 바인딩

Vue 인스턴스 → Template  : 단방향 데이터 바인딩

Vue 인스턴스 ⇄ Template  : 양방향 데이터 바인딩  
(템플릿이란 뷰로 화면을 조작하기위해 제공되는 문법)

대표적인 예  : `input` 태그

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <input type="text" v-model="name">
  {{ name }}
</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
    name: "Hoza"
  }
})
```

결과 :   
![Untitled](https://user-images.githubusercontent.com/78526031/120056406-dc527f00-c076-11eb-897a-23c603d402fe.png)

-> text box 안의 글씨 변경하면 옆에 있는 글씨도 변동 됨

### v-if

조건부 렌더링, 조건에 맞는 것만 렌더링 해준다

`v-if="조건상태"`

`v-if`, `v-else`, `v-else-if` 사용

v-if 디렉티브는 하나의 태그에만 동작하지 않고 해당 태그의 하위 태그에도 동작

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <p v-if="dog">반려동물은 강아지지!</p>
  <p v-else>무슨소리, 고양이지!</p>

  <button v-on:click="dog = !dog">반대</button>

  <template v-if="dog">
    <p>강아지는 귀여워</p>
  </template>
  <template v-else>
    <p>고양이에게 간택을 받는다는 것은 영광이야</p>
  </template>

</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
    dog: true
  }
})
```

![Untitled (1)](https://user-images.githubusercontent.com/78526031/120056427-11f76800-c077-11eb-9b55-6201eb88018e.png)
![Untitled (2)](https://user-images.githubusercontent.com/78526031/120056434-2176b100-c077-11eb-9ddd-c4ce82cdce23.png)

## v-show

`v-show="조건상태"`

v-if 디렉티브처럼 조건 상태값에 따라 결과값으로 보여지는지 여부가 달라짐.

**v-if** 는 아예 렌더링 되지않고, **v-show**는 코드는 삽입되었지만 display:none 처리 되어 보이지않게 됨

## v-for

`v-for="아이템명 in array"`

`v-for="( item, i ) in list"` - ***item***: 반복되는 하나하나의 요소, ***i***: index

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h1>장 볼 리스트</h1>
  <ul>
    <li v-for="(item, i) in list">{{ item }}({{ i }})</li>
  </ul>
</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
    list: ['소갈비', '감자', '당근', 
					 '무', '깐 밤', '소갈비찜 양념']
  }
})
```

장 볼 리스트

- 소갈비(0)
- 감자(1)
- 당근(2)
- 무(3)
- 깐 밤(4)
- 소갈비찜 양념(5)

string 이 아닌 object array 만들기

string 쓸 때 썼던 `v-for="아이템명 in array"` 

`{{ 아이템명 }}` 방식 쓰면 x → `{{아이템명.속성}}`

```jsx
 <script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h1>여행지 리스트</h1>
  <ul>
    <li v-for="city in travelList">
      {{ city.name }} - {{ city.distance }}
    </li>
  </ul>
</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
    travelList:[
     {name: '강릉', distance: '차로 3시간'},
     {name: '부산', distance: '차로 5시간'}
    ]
  }
})
```

### 여행지 리스트

- 강릉 - 차로 3시간
- 부산 - 차로 5시간

vue는 v-for 디렉티브에는 항상 유니크한 key 값을 선언해줄 것을 권고(변경 용이하게 하기 위함)

위 예제의 `<li v-for="city in travelList">`

→ `<li v-for="city in travelList" :key="city.name">`