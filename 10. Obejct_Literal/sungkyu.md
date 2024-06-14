## 객체 리터럴

### 객체 리터럴에 의한 객체 생성

자바스크립트는 프로토타입 기반 객체지향 언어로서 클래스 기반 객체지향 언어와는 달리 다양한 객체 생성 방법을 지원한다.

- 객체 리터럴
- object 생성자 함수
- 생성자 함수
- object.create 메서드
- 클래스(ES6)

**객체 리터럴**

```jsx
const person = {
  name: "Lee",
  sayHello: function () {
    console.log(`Hello! My name is ${this.name}.`);
  },
};
```

### 메서드

자바스크립트 함수는 객체(일급 객체)로 함수를 값으로 취급할 수 있기 때문에 프로퍼티 값으로 사용할 수 있다.

프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 부른다. 즉, 메서드는 객체에 묶여 있는 함수를 의미한다.

```jsx
const circle = {
  radius: 5,
  getDiameter: function () {
    return 2 * this.radius;
  },
};
```
