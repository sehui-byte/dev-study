`v-bind:class="클래스"`

`v-bind:class="{ 클래스명: 조건 }"`

v-bind:class는 문자열과 객체 또는 배열을 둘 다 받을 수 있습니다. 객체를 받은 경우에는 클래스명은 객체의 속성 키에, 해당 클래스가 적용되야 하는 조건이 속성 값으로 들어갑니다.

### html

```jsx
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
<div id="app">
  <!-- click 시 attachRed true/false 변경 -->
  <!-- :class로 object를 바인딩-->
  <div 
    class="demo"
    @click="attachRed = !attachRed" 
    :class="{red: attachRed, green: !attachRed}"
  >1</div>
  
  <div 
    class="demo"
    :class="color"
  >2</div>
  
  <div 
    class="demo"
    :class="divClasses"
  >3</div>
  
  
  
  <div>
    2번 상자의 색: <input type="text" v-model="color">  
  </div>
</div>
```

### javaScript

```jsx
new Vue({
  el: '#app',
  data: {
    attachRed: false,
    color:'green'
  },
  computed: {
    divClasses: function() {
      return {
        red: this.attachRed,
        green: !this.attachRed
      }
    }
  }

})
```

### css

```css
.demo {
  width: 100px;
  height: 100px;
  background-color:grey;
  display:inline-block;
  margin: 10px;
}

.red {
  background-color: red;
}

.green {
  background-color: green;
}

.blue {
  background-color: blue;
}
```
![Untitled](https://user-images.githubusercontent.com/78526031/120058110-77e9ec80-c083-11eb-947f-7ec2fa9d8949.png)

- 1번 사각형 : v-bind:class 가 객체를 받은 경우

```html
<div 
    class="demo"
    @click="attachRed = !attachRed" 
    :class="{red: attachRed, green: !attachRed}" >1</div>
```

`@click`은   `v-on:click`의 약어

:class 는 v-bind:class의 약어

`v-bind:class="클래스명"`

`v-bind:class="{ 클래스명: 조건 }"` 이기 때문에

여기서의 **red** , **green** 은 class 명

- html 2번 사각형, v-bind:class가 문자열을 받는 경우

```html
<div 
  class="demo"
  :class="color" >2</div>
```

```html
<div>
  2번 상자의 색: <input type="text" v-model="color">  
</div>
```

텍스트박스안에 입력하는 문자 color에 바인딩 됨

`:class="문자열"` → 조건에 상관없이 문자열 그대로 클래스에 적용

- html 3번 사각형, v-bind:class의 객체 computed

```jsx
divClasses: function() {
  return {
    red: this.attachRed,
    green: !this.attachRed
  }
}
```

computed 내의 divClasses는 1번 사각형과 같은 객체를 return.

computed나 methods에서 반환되는 객체 혹은 문자열을 클래스로 바인딩 시킬 수 있다

스타일 바인딩은 거의 CSS 처럼 보이지만 JavaScript 객체

`v-bind:style="{ 스타일명: 스타일값, 스타일명: 스타일값}"`

2번박스 수정

```html
<div 
  class="demo"
  :class="color"
  :style="{height: myHeight}"
>2</div>
```

computed에 추가

```jsx
myHeight: function () {
   return this.attachRed ? '50px' : '200px'
}
```

→ 박스 빨간색일때 50px, 초록색일때 200px
![Untitled (1)](https://user-images.githubusercontent.com/78526031/120058382-715c7480-c085-11eb-9643-56904ecf4a48.png)
![Untitled (2)](https://user-images.githubusercontent.com/78526031/120058395-8cc77f80-c085-11eb-936a-466b68108d6c.png)