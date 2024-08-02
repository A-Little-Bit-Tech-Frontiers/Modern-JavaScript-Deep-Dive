## 19 프로토타입

## 19.2 상속과 프로토타입

자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거한다. → **자바스크립트는 프로토타입을 기반으로 상속을 구현한다.**

```jsx
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    // Math.PI는 원주율을 나타내는 상수다.
    return Math.PI * this.radius ** 2;
  };
}

// 반지름이 1인 인스턴스 생성
const circle1 = new Circle(1);
// 반지름이 2인 인스턴스 생성
const circle2 = new Circle(2);

// Circle 생성자 함수는 인스턴스를 생성할 때마다 동일한 동작을 하는
// getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.
// getArea 메서드는 하나만 생성하여 모든 인스턴스가 공유해서 사용하는 것이 바람직하다.
console.log(circle1.getArea === circle2.getArea); // false

console.log(circle1.getArea()); // 3.141592653589793
console.log(circle2.getArea()); // 12.566370614359172
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/af07afc3-ce9f-4078-af0c-58894664ff01/Untitled.png)

Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 상위(부모) 객체 역할을 하는 Circle.prototype의 모든 프로퍼티와 메서드를 상속받는다.

## 19.3 프로토타입 객체

프로토타입 객체(또는 줄여서 프로토타입)란 객체지향 프로그래밍의 근간을 이루는 객체 간 상속을 구현하기 위해 사용된다. 프로토타입은 어떤 객체의 상위(부모) 객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)를 제공한다. 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티처럼 자유롭게 사용할 수 있다.

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조(null인 경우도 있다)다. [[Prototype]]에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다. 즉, 객체가 생성될 때 객체 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장된다.

→ 모든 객체는 하나의 프로토타입을 갖는다. 그리고 모든 프로토타입은 생성자 함수와 연결되어 있다. 즉, 객체와 프로토타입과 생성자 함수는 아래 그림과 같이 서로 연결되어 있다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/abfda179-46bc-48ea-aa4f-828a0a218aed/Untitled.png)

[[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만, 위 그림처럼 **proto** 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 자신의 [[Prototype]] 내부 슬롯이 가리키는 프로토타입에 간접적으로 접근할 수 있다. 그리고 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.

### 19.3.1 **proto** 접근자 프로퍼티

모든 객체는 **proto** 접근자 프로퍼티를 통해 자신의 프로토타입, 즉 [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.

****proto**는 접근자 프로퍼티다.**

```jsx
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

****proto** 접근자 프로퍼티는 상속을 통해 사용된다.**

**proto** 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티다. 모든 객체는 상속을 통해 Object.prototype.**proto** 접근자 프로퍼티를 사용할 수 있다.

```jsx
const person = { name: "Lee" };

// person 객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(person.hasOwnProperty("__proto__")); // false

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티다.
console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); // true
```

****proto** 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**

[[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조(순환 참조)에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다.

```jsx
const parent = {};
const child = {};

// child의 프로토타입을 parent로 설정
child.__proto__ = parent;
// parent의 프로토타입을 child로 설정
parent.__proto__ = child; // TypeError: Cyclic __proto__ value
```

위의 예제 코드에서 parent 객체를 child 객체의 프로토타입으로 설정한 후, child 객체를 parent 객체의 프로토타입으로 설정했다. 이러한 코드가 에러 없이 정상적으로 처리되면 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인이 만들어지기 때문에 **proto** 접근자 프로퍼티는 에러를 발생시킨다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/277e2ef4-016a-4ea7-9e0a-1ca8e9c934f6/Untitled.png)

프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다. 즉, 프로퍼티 검색 방향이 한쪽 방향으로만 흘러가야 한다. 하지만 위 그림과 같이 서로가 자신의 프로토타입이 되는 비정상적인 프로토타입 체인, 다시 말해 순환 참조하는 프로토타입 체인이 만들어지면 프로토타입 체인 종점이 존재하지 않기 때문에 프로토타입 체인에서 프로퍼티를 검색할 때 무한 루프에 빠진다. 따라서 아무런 체크 없이 무조건적으로 프로토타입을 교체할 수 없도록 **proto** 접근자 프로퍼티를 통해 프로토타입에 접근하고 교체하도록 구현되어 있다.

****proto** 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장하지 않는다.**

모든 코드 내에서 **proto** 접근자 프로퍼티를 직접 사용하는 것은 권장하지 않는다. 모든 객체가 **proto** 접근자 프로퍼티를 사용할 수 있는 것은 아니기 때문이다. → 직접 상속을 통해 Object.prototype을 상속받지 않는 객체를 생성할 수도 있기 때문에 **proto** 접근자 프로퍼티를 사용할 수 없는 경우가 있다.

```tsx
/ obj는 프로토타입 체인의 종점이다. 따라서 Object.__proto__를 상속받을 수 없다.
const obj = Object.create(null);

// obj는 Object.__proto__를 상속받을 수 없다.
console.log(obj.__proto__); // undefined

// 따라서 Object.getPrototypeOf 메서드를 사용하는 편이 좋다.
console.log(Object.getPrototypeOf(obj)); // null
```

따라서 **proto** 접근자 프로퍼티 대신 프로토타입의 참조를 취득하고 싶은 경우에는 Object.getPrototypeOf 메서드를 사용하고, 프로토타입을 교체하고 싶은 경우에는 Object.setPrototypeOf 메서드를 사용할 것을 권장한다.

```tsx
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

### 19.3.2 함수 객체의 prototype 프로퍼티

**함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**

```tsx
// 함수 객체는 prototype 프로퍼티를 소유한다.
(function () {}).hasOwnProperty("prototype"); // -> true

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
({}).hasOwnProperty("prototype"); // -> false
```

생성자 함수로서 호출할 수 없는 함수, 즉 non-constructor인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드는 prototype 프로퍼티를 소유하지 않으며 프로토타입도 생성하지 않는다.

```tsx
// 화살표 함수는 non-constructor다.
const Person = (name) => {
  this.name = name;
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(Person.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(Person.prototype); // undefined

// ES6의 메서드 축약 표현으로 정의한 메서드는 non-constructor다.
const obj = {
  foo() {},
};

// non-constructor는 prototype 프로퍼티를 소유하지 않는다.
console.log(obj.foo.hasOwnProperty("prototype")); // false

// non-constructor는 프로토타입을 생성하지 않는다.
console.log(obj.foo.prototype); // undefined
```

모든 객체가 가지고 있는(엄밀히 말하면 Object.prototype으로부터 상속받은) **proto** 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다. 하지만 이들 프로퍼티는 사용하는 주체가 다르다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/3ac7ce46-eb1f-4d1d-87e0-6c5cc464ddce/Untitled.png)

```tsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/96105898-f554-4c27-a316-db32ea32f754/Untitled.png)

### 19.3.3 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 constructor 프로퍼티는 prototype 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다. 이 연결은 생성자 함수가 생성될 때, 즉 함수 객체가 생성될 때 이뤄진다.

```tsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/82348127-17e2-4acf-bf6d-b91b2fe06763/Untitled.png)

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재한다. 하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다

Object 생성자 함수에 인수를 전달하지 않거나 undefined 또는 null을 인수로 전달하면서 호출하면 내부적으로 추상 연산 OrdinaryObjectCreate를 호출하여 Object.prototype을 프로토타입으로 갖는 빈 객체를 생성한다.

```tsx
// obj 객체는 Object 생성자 함수로 생성한 객체가 아니라 객체 리터럴로 생성했다.
const obj = {};

// 하지만 obj 객체의 생성자 함수는 Object 생성자 함수다.
console.log(obj.constructor === Object); // true
```

> 추상 연산은 ECMAScript 사양에서 내부 동장의 구현 알고리즘을 표현한 것이다. ECMAScript 사양에서 설명을 위해 사용되는 함수와 유사한 의사 코드라고 이해하자.

리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴 표기법에 의해 생성된 객체도 가상적인 생성자 함수를 갖는다. 프로토타입은 생성자 함수와 더불어 생성되며 prototype, constructor 프로퍼티에 의해 연결되어 있기 때문이다. 다시 말해, **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.**

객체 리터럴에 의해 생성한 객체와 Object 생성자 함수에 의해 생성한 객체는 생성 과정에 미묘한 차이는 있지만 결국 객체로서 동일한 특성을 갖는다. 함수 리터럴에 의해 생성한 함수와 Function 생성자 함수에 의해 생성한 함수는 생성 과정과 스코프, 클로저 등의 차이가 있지만 결국 함수로서 동일한 특성을 갖는다.

따라서 프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 생각해도 크게 무리는 없다. 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토 타입은 아래와 같다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/b3790ac3-686b-4343-9fec-3874be38444b/Untitled.png)

## 19.5 프로토타입의 생성 시점

리터럴 표기법에 의해 생성된 객체도 생성자 함수와 연결되는 것을 확인하였고, 객체는 리터럴 표기법 또는 생성자 함수에 의해 생성되므로 결국 모든 객체는 생성자 함수와 연결되어 있다.

→ 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다. → 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문이다.

### 19.5.1 사용자 정의 생성자 함수와 프로토타입 생성 시점

**생성자 함수로서 호출할 수 있는 함수, 즉 constructor는 함수 정의가 평가되어 있는 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.**

함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행된다. → 이때 프로토타입도 더불어 생성된다.

생성된 프로토타입은 오직 constructor 프로퍼티만을 갖는 객체다. 프로토타입도 객체이고 모든 객체는 프로토타입을 가지므로 프로토타입도 자신의 프로토타입을 갖는다. 생성된 프로토타입의 프로토타입은 Object.prototype이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/50f92f0d-6be2-42df-8af0-fafead7d29b3/Untitled.png)

이처럼 빌트인 생성자 함수가 아닌 사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입도 더불어 생성되며, 생성된 프로토타입의 프로토타입은 언제나 Object.prototype이다.

### 19.5.2 빌트인 생성자 함수와 프로토타입 생성 시점

`Object`, `Striong`, `Number`, `Function`, `Array`, `RegExp`, `Date`, `Promise`등과 같은 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.

**모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성**된다. 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/bc9b4c78-5180-43ed-bb9e-35c9d45c48d3/Untitled.png)

이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. **이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 `[[Prototype]]` 내부 슬롯에 할당된다.** 이로써 생성된 객체는 프로토타입을 상속받는다.

## 19.6 객체 생성 방식과 프로토타입의 결정

- 객체 리터럴
- `Object`생성자 함수
- 생성자 함수
- `Object.create`메서드
- 클래스(ES6)

위와 같이 다양한 방식으로 생성된 모든 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산 OrdinaryObjectCreate에 의해 생성된다는 공통점이 있다.

추상 연산 OrdinaryObjectCreate는 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달받는다. 그리고 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달할 수 있다.

- 추상 연산 OrdinaryObjectCreate는 빈 객체를 생성한 후,
- 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 프로퍼티를 객체에 추가한다.
- 그리고 인수로 전달받은 프로토타입을 자신이 생성한 객체 `[[Prototype]]` 내부 슬롯에 할당한 다음
- 생성한 객체를 반환한다.

→ 즉, **프로토타입은 추상 연산 OrdinaryObjectCreate에 전달되는 인수에 의해 결정된다.이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정**된다.

### 19.6.1 객체 리터럴에 의해 생성된 객체의 프로토타입

자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 OrdinaryObjectCreate를 호출한다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 Object.prototype이다. 즉 객체 리터럴에 의해 생성되는 객체의 프로토타입은 Object.prototype이다.

```tsx
const obj = { x: 1 };
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/4f9d8944-4c5e-4aca-94e2-5bae41276d8a/Untitled.png)

### 19.6.2 Object 생성자 함수에 의해 생성된 객체의 프로토타입

Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성된다. Object 생성자 함수를 호출하면 객체 리터럴과 마찬가지고 추상 연산 OrdinaryObjectCreate가 호출된다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 포토타입은 Object.prototype이다. 즉, Object 생성자 함수에 의해 생성되는 객체의 프로토타입은 Object.prototype이다

```tsx
const obj = new Object();
obj.x = 1;
```

위 코드가 실행되면 추상 연산 OrdinaryObjectCreate에 의해 다음과 같이 Object 생성자 함수와 Object.prototype과 생성된 객체 사이에 연결이 만들어진다.(객체리터럴과 동일구조)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/b84b7b85-aacb-4ccb-a622-78ad9dfc9923/Untitled.png)

객체 리터럴과 Object 생성자 함수에 의한 객체 생성 방식의 차이는 **프로퍼티를 추가하는 방식**에 있다. 객체 리터럴 방식은 객체 리터럴 내부에 프로퍼티를 추가하지만 Object 생성자 함수 방식은 일단 빈 객체를 생성한 이후에 프로퍼티를 추가해야 한다.

### 19.6.3 생성자 함수에 의해 생성된 객체의 프로토타입

`new`연산자와 함께 생성자 함수를 호출하여 객체를 생성하는 방식도 마찬가지로 추상연산`OrdinaryObjectCreate`가 호출된다. 이때 추상 연산 OrdinaryObjectCreate에 전달되는 프로토타입은 생성자 함수 prototype 프로퍼티에 바인딩되어 있는 객체다. 즉, 생성자 함수에 의해 생성되는 객체의 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체다.

```tsx
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");
```

위 코드가 실행되면 추상 연산 OrdinaryObjectCreate에 의해 다음과 같이 **생성자 함수**와 **생성자 함수의 prototype 프로퍼티에 바인딩되어있는 객체**와 **생성된 객체**사이에 연결이 만들어진다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/33c48dfd-9739-4474-ac11-b5eabeb3d634/Untitled.png)

표준 빌트인 객체인 Object 생성자 함수와 더불어 생성된 프로토타입 Object.prototype은 다양한 빌트인 메서드(hasOwnProperty, prototypeIsEnumerable 등)를 갖고 있다. 하지만 사용자 정의 생성자 함수 Person과 더불어 생성된 프로토타입 Person.prototype의 프로퍼티는 constructor뿐이다.

프로토타입 Person.prototype에 프로퍼티를 추가하여 하위(자신) 객체가 상속받을 수 있도록 구현해보면 아래와 같다.

프로토타입은 객체다. 따라서 일반 객체와 같이 프로토타입에 프로퍼티를 추가/삭제할 수 있다. 그리고 이렇게 추가/삭제된 프로퍼티는 프로토타입 체인에 즉각 반영된다.

```tsx
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

const me = new Person("Lee");
```

Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아 자신의 메서드처럼 사용할 수 있다.

## 19.7 프로토타입 체인

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/3a426643-e446-4d1c-9d8a-4758671dfcf3/Untitled.png)

자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입 프로퍼티를 순차적으로 검색한다. 이를 **프로토타입 체인**이라 한다. 프로토타입 체인은 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.

**Object,prototype을 프로토타입 체인의 종점이라 한다.** Object.prototype의 프로토타입, 즉 [[Prototype]] 내부 슬롯의 값은 null이다.

프로토타입 체인의 종점인 Object.prototype에서도 프로퍼티를 검색할 수 없는 경우 undefined를 반환한다. 이때 에러가 발생하지 않는다.

→ 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘

→ 스코프 체인은 식별자 검색을 위한 메커니즘

**스코프 체인과 프로토타입 체인은 서로 연관없이 별로도 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 데 사용된다.**

## 19.8 오버라이딩과 프로퍼티 섀도잉

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/e5c06c7b-f80b-42e8-9404-e6f3565f3e5b/Untitled.png)

프로토타입이 소유한 프로퍼티(메서드 포함)를 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티를 인스턴스 프로퍼티라고 부른다.

프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 프로토타입 프로퍼티를 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라 인스턴스 프로퍼티로 추가한다. 이때 인스턴스 메서드 sayHello는 프로토타입 메서드 sayHello를 오버라이딩했고 프로토타입 메서드 sayHello는 가려진다. 이처럼 상속 관계에 의햬 프로퍼티가 가려지는 현상을 프로퍼티 섀도잉이라 한다.

<aside>
💡 오버라이딩
상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용하는 방식이다.

</aside>

<aside>
💡 오버로딩
함수의 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드를 구현하고 매개변수에 의해 메서드를 구별하여 호출하는 방식이다. 자바스크립트는 오버로딩을 지원하지 않지만 arguments 객체를 사용하여 구현할 수는 있다.

</aside>

하위 객체를 통해 프로토타입의 프로퍼티를 변경 또는 삭제하는 것은 불가능하다. → 하위 객체를 통해 프로토타입에 get 액세스는 허용되나 set 액세스는 허용되지 않는다.

프로토타입 프로퍼티를 변경 또는 삭제하려면 하위 객체를 통해 프로토타입 체인으로 접근하는 것이 아니라 프로토타입에 직접 접근해야 한다.

```jsx
// 프로토타입 메서드 변경
Person.prototype.sayHello = function () {
  console.log(`Hey! My name is ${this.name}`);
};

me.sayHello(); // Hey! My name is Lee

// 프로토타입 메서드 삭제
delete Person.prototype.sayHello;
me.sayHello(); // TypeError: me.sayHello is not a function
```

## 19.9 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경할수 있다. 이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미한다. 이러한 특징을 활용하여 **객체 간의 상속 관계를 동적으로 변경**할 수 있다. 프로토타입은 생성자 함수 또는 인스턴스에 의해 교체할 수 있다.

### 19.9.1 생성자 함수에 의한 프로토타입의 교체

```jsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 1️⃣ 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");
```

1️⃣에서 `Person.prototype`에 객체 리터럴을 할당했다. 이는 **`Person` 생성자 함수가 생성할 객체의 프로토타입을 객체 리터럴로 교체**한 것이다

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/f0adc0c0-c756-4ae0-bb2d-24f9295d5cff/Untitled.png)

프로토타입으로 교체한 객체 리터럴에는 `constructor` 프로퍼티가 없다. `constructor` 프로퍼티는 자바스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티다. 따라서, `me` 객체의 생성자 함수를 검색하면 `Person`이 아닌 `Object`가 나온다.

```jsx
//constructor 프로퍼티와 생성자 함수간의 연결 파괴
console.log(me.constructor === Person); // false
console.log(me.constructor === Object); // true
```

이처럼 **프로토타입을 교체하게 되면 `constructor`프로퍼티와 생성자 함수간의 연결이 파괴되는데,`constructor` 프로퍼티를 추가하여 프로토타입의 `constructor` 프로퍼티를 되살린다.**

```jsx
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    //constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");

//constructor 프로퍼티와 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```

### 19.9.2 인스턴스에 의한 프로토타입의 교체

프로토타입은 생성자 함수의 `prototype` 프로퍼티 뿐만 아니라 인스턴스의 `__proto__` 접근자 프로퍼티(또는 `Oject.getPrototypeOf` 메서드)를 통해 접근할 수 있고 이를 통해 프로토타입을 교체할 수 있다. 생성자 함수의 `prototype` 프로퍼티에 다른 임의의 객체를 바인딩 하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체하는 것이다. `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것이다.

```jsx
function Person(name) {
  this.name = name;
}

const me = new Person("april");

// 프로토타입으로 교체할 객체
const parent = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
  constructor: Person,
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
Person.prototype = parent;

// 1️⃣ me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is april
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/6ce677a5-5578-42a9-bc66-3e1efdb64ba7/Untitled.png)

프로토타입으로 교체한 객체에는 constructor 프로퍼티가 없으므로 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다. 따라서 프로토타입의 constructor 프로퍼티로 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다.

## 19.10 instanceof 연산자

instanceof 연산자는 이항 연산자로서 좌변에 객체를 가리키는 식별자, 우변에 생성자 함수를 가리키는 식별자를 피연산자로 받는다. 만약 우변의 피연산자가 함수가 아닌 경우 TypeError가 발생한다.

```jsx
객체 instanceof 생성자 함수
```

**우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로 평가되고, 그렇지 않은 경우에는 false로 평가된다.**

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// Person.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Person); // true

// Object.prototype이 me 객체의 프로토타입 체인 상에 존재하므로 true로 평가된다.
console.log(me instanceof Object); // true
```

이처럼 instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.

## 19.11 직접 상속

### 19.11.1 Object.create에 의한 직접 상속

`Object.create`메서드는 명시적으로 **프로토타입을 지정**하여 새로운 객체를 생성한다. Object.create 메서드도 다른 객체 생성 방식과 마찬가지로 추상 연산 OrdinaryObjectCreate를 호출한다.

Object.create 메서드의 첫 번째 매개변수에는 생성할 객체의 프로토타입으로 지정할 객체를 전달한다. 두번째 매개변수에는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다.

```jsx
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {};와 동일하다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj → Object.prototype → null
// obj = { x: 1 };와 동일하다.
obj = Object.create(Object.prototype, {
  x: { value: 1, writable: true, enumerable: true, configurable: true },
});
// 위 코드는 다음과 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일하다.
obj = Object.create(Person.prototype);
obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true
```

객체를 생성하면서 직접적으로 상속을 구현하는 것으로 이 메서드의 장점은 다음과 같다.

- `new` 연산자 없이도 객체를 생성할 수 있다.
- 프로토타입을 지정하면서 객체를 생성할 수 있다.
- 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

ESLint에서는 Object.prototype의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는다. 그 이유는 Object.create 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문이다. 프로토타입 체인의 종점에 위치하는 객체는 Object.prototype의 빌트인 메서드를 사용할 수 없다.

```jsx
// 프로토타입이 null인 객체, 즉 프로토타입 체인의 종점에 위치하는 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

console.log(Object.getPrototypeOf(obj) === null); // true

// obj는 Object.prototype의 빌트인 메서드를 사용할 수 없다.
console.log(obj.hasOwnProperty("a")); // TypeError: obj.hasOwnProperty is not a function
```

따라서 이 같은 에러를 발생시킬 위험을 없애기 위해 Object.prototype의 빌트인 메서드는 아래와 같이 간접적으로 호출하는 것이 좋다.

```jsx
// 프로토타입이 null인 객체를 생성한다.
const obj = Object.create(null);
obj.a = 1;

// console.log(obj.hasOwnProperty('a')); // TypeError: obj.hasOwnProperty is not a function

// Object.prototype의 빌트인 메서드는 객체로 직접 호출하지 않는다.
console.log(Object.prototype.hasOwnProperty.call(obj, "a")); // true
```

### 19.11.2 객체 리터럴 내부에서 **proto**에 의한 직접 상속

ES6에서는 객체 리터럴 내부에서 **proto** 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

```jsx
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};
/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

## 19.12 정적 프로퍼티/메서드

```jsx
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// ① 정적 프로퍼티
Person.staticProp = "static prop";

// ② 정적 메서드
Person.staticMethod = function () {
  console.log("staticMethod");
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// ③ 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

Person 생성자 함수는 객체이므로 자신의 프로퍼티/메서드를 소유할 수 있다. Person 생성자 함수 객체가 소유한 프로퍼티/메서드를 정적 프로퍼티/메서드라고 한다. 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출 할 수 없다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/1d9105f8-0a00-4886-8c38-93f10f400b53/Untitled.png)

생성자 함수가 생성한 인스턴스는 자신의 프로토타입 체인에 속한 객체의 프로퍼티/메서드에 접근할 수 있다. 하지만 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니므로 인스턴스로 접근할 수 없다.

앞에서 살펴본 Object.create 메서드는 Object 생성자 함수의 정적 메서드고 Object.prototype.hasOwnProperty 메서드는 Object.prototype의 메서드다. 따라서 Object.create 에서만드는 인스턴스, 즉 Object 생성자 함수가 생성한 객체로 호출할 수 없다. 하지만 Object.prototype.hasOwnProperty 메서드는 모든 객체의 프로토타입 체인의 종점, 즉 Object.prototype의 메서드이므로 모든 객체가 호출할 수 있다.

만약 인스턴스/프로토타입 메서드 내에서 this를 사용하지 않는다면 그 메서드는 정적 메서드로 변경할 수 있다. 인스턴스가 호출한 인스턴스/프로토타입 메서드 내에서 this는 인스턴스를 가리킨다. 메서드 내에서 인스턴스를 참조할 필요가 없다면 정적 메서드로 변경하여도 동작한다. 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 하지만 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.

```jsx
function Foo() {}

// 프로토타입 메서드
// this를 참조하지 않는 프로토타입 메소드는 정적 메서드로 변경해도 동일한 효과를 얻을 수 있다.
Foo.prototype.x = function () {
  console.log("x");
};

const foo = new Foo();
// 프로토타입 메서드를 호출하려면 인스턴스를 생성해야 한다.
foo.x(); // x

// 정적 메서드
Foo.x = function () {
  console.log("x");
};

// 정적 메서드는 인스턴스를 생성하지 않아도 호출할 수 있다.
Foo.x(); // x
```

프로토타입 프로퍼티/메서드를 표기할 때 `prototype`을 `#`으로 표기하는 경우도 있다.

- 예시) `Object.prototype.isPrototypeof`를 `Object#isPrototypeof` 으로 표기

## 19.13 프로퍼티 존재 확인

in 연산자는 확인 대상 객체의 프로퍼티뿐만 아니라 확인 대상 객체가 상속받은 모든 프로토타입의 프로퍼티를 확인하므로 주의가 필요하다.

in 연산자 대신 ES6에서 도입된 Reflect.has 메서드를 사용할 수도 있다. Reflect.has 메서드는 in 연산자와 동일하게 동작한다.

```tsx
const person = { name: "Lee" };

console.log(Reflect.has(person, "name")); // true
console.log(Reflect.has(person, "toString")); // true
```

### 19.13.2 Object.prototype.hasOwnProperty 메서드

Object.prototype.hasOwnProperty 메서드를 사용해도 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.

## 19.14 프로퍼티 열거

for…in 문은 in 연산자처럼 순회 대상 객체의 프로퍼티뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거한다. 하지만 toString과 같은 Object.prototype의 프로퍼티가 열거되지 않는다.

→ **for…in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하며 열거한다.**
