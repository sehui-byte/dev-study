## Vue Template

**템플릿**이란 뷰로 화면을 조작하기위해 제공되는 문법이다.

뷰 인스턴스에서 관리하는 데이터를 화면에 연결하는 데이터 바인딩 '`{{ }}'`  과 화면의 조작을 편하게 할 수 있는 디렉티브 '`<v->`'로 나뉜다.

methods 객체를 이용하지 않고 직접 JavaScript 코드를 template 내에 입력 해 볼 차례

### Directive에 JS코드 넣기

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="dd">
{{count}}
<button v-on:click="count++">클릭</button>
```

```jsx
new Vue({
	el: "#dd",
  data : {
  	count:0
  }
})
```

### {{mustache}}에 js코드 넣기

코드는 동일 `{{count}}`만 → `{{count*3}}` 으로 바꿈


## Computed

너무 많은 연산을 템플릿 안에서 하면 코드가 비대해지고 유지보수가 어려움 

→ computed 사용

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
 {{count *3 }} <br>
  {{ isLargerThanTen }}
  <button v-on:click="count ++">클릭</button>
</div>
```

```jsx
new Vue({ 
  el: "#app", 
  data: {
    count: 0
  },
  computed: {
    isLargerThanTen: function () {
      return this.count * 3 > 10 ? "10보다 큽니다." : "10보다 작거나 같습니다."
    }
  }
})
```

`computed:` → `methods:` 로 바꾸고  `{{ isLargerThanTen }}` →  `{{ isLargerThanTen() }}` 로 바꾸면 methods 됨

computed 와 method(함수) 결과는 같음 

그러나 **computed**속성은 종속 대상을 따라 저장(캐싱)됨.→ 자기가 참고한 값이 바뀔 때만 바뀜

**method**는 호출하면 렌더링 다시 할 때마다 항상 함수 실행 함.

## Watch

**watch** : 기존 Vue 인스턴스 내에 선언된 값의 변화를 감시

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h1>{{ count }}</h1><br>
  <p>{{ text }}</p>
  <button @click="count--">카운트 감소</button>
</div>
```

```jsx
new Vue({
	el: "#app",
  data: {
    ***count***: 3,
    text: '변경 전입니다.'
  },
  watch: {
    ***count***: function (newVal, oldVal) {
      this.text = oldVal + '에서 ' + newVal + '로 변경되었습니다.'
    }
  }
})
```

watch는 Vue 인스턴스 내에 선언된 값 그대로 다시 사용

→ data에 선언되어 있는 ***count***가 watch 안에도 그대로 선언됨

watch는 감시하고 있는 대상의 변경된 값과 이전 값을 인자로 받을 수 있다.

→ `watch: {***count***: function (**newVal, oldVal**) { } }` 에서 알 수 있음

## computed vs watch

**computed** : 참조하고 있는 값 변경될 때마다 정의된 계산식에 따라 값 출력

**watch** : 값이 변경될 때 실행되는 함수 지정 가능

→ computed : 계산된 값 출력 용, watch : 조건에 따른 함수 실행

```html
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <h1>{{ count }}</h1><br>
  <!--computed의 calculated안의 함수를 실행시키기 위해서는
 calculated를 실제로 출력해야 합니다.-->
  <!-- {{ calculated }} -->
  <button @click="count--">카운트 감소</button>
</div>
```

```jsx
new Vue({
	el: "#app",
  data: {
    count: 3,
  },
  computed: {
    count: function () {
    	if(this.count === 2) {
      	alert('값이 2가 되었습니다.')
      }
    }
  },
  watch: {
    count: function (newVal) {
      if(newVal === 0) {
      	alert('값이 0이 되었습니다.')
        this.count = 3
      }
    }
  }
})
```

→ 버튼눌러서 숫자 감소해서 2가 되었을 때 아무일도 안일어남 
→ 0이 되면 alert 발생됨

calculated안에 alert창을 띄우고 싶다면 HTML 코드 안에 {{ calculated }}를 넣어서 실제로 출력을 해야만 calculated 안에 정의된 함수 실행 됨

## multiple instance 여러인스턴스 사용

```html
<script src="https://npmcdn.com/vue/dist/vue.js"></script>

<div id="app">
  {{ title }}
</div>

<div id="app2">
  {{ title }}
  <button @click="onChange">변경</button>
</div>
```

```jsx
var vm1 = new Vue({ 
// vm은 vue model 통용되는 약어, 아무 변수나 사용해도됨
  el: '#app',
  data: {
    title: '첫 인스턴스'
  }
})

setTimeout(function() {
  vm1.title = 'Changed By Timer'
}, 3000)
//Vue 인스턴스끼리가 아닌 외부에서도 해당 vm1 사용가능

var vm2 = new Vue({
  el: '#app2',
  data: {
    title: '두번째 인스턴스'
  },
  methods:{
    onChange: function () {
    	vm1.title = '변경됨'
    }
  }
})
```

```jsx
var createdData = {
 title: '첫 인스턴스'
}

var vm1 = new Vue({ 
  el: '#app',
  data: createdData
})

var vm2 = new Vue({
  el: '#app2',
  data: {
    title: '두번째 인스턴스'
  },
  methods:{
    onChange: function () {
    	vm1.title = '변경됨'
    }
  }
})
```