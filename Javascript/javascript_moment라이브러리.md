

### Moment.js 쓸 때 주의할 점

- moment.js에서 `add()`나 `subtract()` 같은 경우 원래 값을 변경하기 때문의 사용시 주의해야 한다.

```javascript

const d1 = moment();
console.log(d1.format('YYYY-MM-DD'));

//shllow copy
//subtract, add() 는 d1의 값을 바꿔버린다
//d1.subtract(1, 'days');
//console.log(d1.format('YYYY-MM-DD'));


const d2 = d1.clone();
console.log(d2.subtract(1,'days').format('YYYY-MM-DD'));
```

![image-20210608223520986](C:\Users\김세희\AppData\Roaming\Typora\typora-user-images\image-20210608223520986.png)

----

### 참고자료

- https://jsdev.kr/t/moment-js/1826

- https://momentjs.com/guides/#/lib-concepts/mutability/

