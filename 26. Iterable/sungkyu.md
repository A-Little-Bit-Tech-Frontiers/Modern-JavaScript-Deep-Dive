## 34.1 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션(자료구조)을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.

ES6 이전의 순회 가능한 데이터 컬렉션, 즉 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등은 통일된 규약 없이 각자 나름의 구조를 가지고 for 문, for … in문, forEach 메서드 등 다양한 방법으로 순회할 수 있었다. ES6에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for … of문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화했다.

- **이터러블 프로토콜**

Well-known Symbol인 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속 받은 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이러한 규약을 이터러블 프로토콜이라 하며, **이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 이터러블은 for … of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.**

- **이터레이터 프로토콜**

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. 이터레이터는 next 메서드를 소유하며 next 메서드를 호출하면 이터러블을 순회하며 value와 done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환한다. 이러한 규약을 이터레이터 프로토콜이라 하며, **이터레이터 프로토콜을 준수한 객체를 이터레이터라 한다.** 이터레이터는 이터러블의 요소를 탐색하기 위한 포인터 역할을 한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/76b52bcf-c023-4c0c-855c-dfc19f962053/image.png)

### 34.1.1 이터러블

이터러블 프로토콜을 준수한 객체를 이터러블이라 한다. 즉, 이터러블은 Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말한다.

```jsx
const isIterable = (v) =>
  v !== null && typeof v[Symbol.iterator] === "function";

// 배열, 문자열, Map, Set 등은 이터러블이다.
isIterable([]); // true
isIterable(""); // true
isIterable(new Map()); // true
isIterable(new Set()); // true
isIterable({}); // false
```

Symbol.iterator 메서드를 직접 구현하지 않거나 상속받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다. 따라서 일반 객체는 for … of 문으로 순회할 수 없다.

```jsx
//유사 배열 객체는 이터러블이 아니기 때문에 for ... of 문으로 순회할 수 없다.
for (const item of arrayLike) {
  console.log(item);
}
// is not iterable

// 이터러블이 아닌 일반 객체는 배열 디스트럭처링 할당의 대상으로 사용할 수 없다.
const [a, b] = obj; // TypeError: obj is not iterable
```

### 34.1.2 이터레이터

이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. **이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.**

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터리이터를 반환한다.
const iterator = array[Symbol.iterator]();

// Symbol.iterator 메서드가 반환한 이터리이터는 next 메서드를 갖는다.
console.log("next" in iterator); // true
```

이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 즉, next 메서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 순회 결과를 나타내는 **이터레이터 리절트 객체를 반환한다.**

```jsx
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메서드는 이터리이터를 반환한다. 이터레이터는 next 메서드를 갖는다.
const iterator = array[Symbol.iterator]();

// 이터레이터 리절트 객체는 value 와 done 프로퍼티를 갖는 객체다.
console.log(iterator.next()); // { value: 1, done: false }
console.log(iterator.next()); // { value: 2, done: false }
console.log(iterator.next()); // { value: 3, done: false }
console.log(iterator.next()); // { value: undefined, done: true }
```

## 34.2 빌트인 이터러블

자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다. 다음과 같은 표준 빌트인 객체들은 빌트인 이터러블이다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/df3fa812-89d5-47ce-893b-5b2f4bb0ca7d/image.png)

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/4524f4c9-fb52-4d2a-873c-c6bdee120109/image.png)

## 34.3 for..of 문

`for (변수선언문 of 이터러블) {…}`

`for (변수선언문 in 객체) {…}`

for..in문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]]의 값이 true인 프로퍼티를 순회하여 열거한다. 이때 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.

for…of 문은 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 메서드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for…of 문의 변수에 할당한다.

## 34.4 이터러블과 유사 배열 객체

유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다. 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for문으로 순회할 수 있고, 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로 가지므로 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.

```jsx
// 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

// 유사 배열 객체는 length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있다.
for (let i = 0; i < arrayLike.length; i++) {
  // 유사 배열 객체는 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
  console.log(arrayLike[i]); // 1 2 3
}
```

유사 배열 객체는 이터러블이 아닌 일반 객체다. 따라서 유사 배열 객체에는 Symbol.iterator 메서드가 없기 때문에 for…of 문으로 순회할 수 없다.

```jsx
//유사 배열 객체는 이터러블이 아니기 때문에 for...of문으로 순회할 수 없다.
for (const item of arrayLike) {
  console.log(item); // TypeError
}
```

단, arguments, NodeList, HTMLCollection은 유사 배열 객체이면서 이터러블이다.

## 34.5 이터레이션 프로토콜의 필요성

이터러블은 for…of 문, 스프레드 문법, 배열 디스트럭처링 할당과 같은 데이터 소비자에 의해 사용되므로 데이터 공급자의 역할을 한다고 할 수 있다.

다양한 데이터 공급자가 이터레이션 프로토콜을 준수하도록 규정하면 데이터 소비자는 이터레이션 프로토콜만 지원하도록 구현하면 된다.

→ 이처럼 이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 데이터 소비자와 **데이터 공급자를 연결하는 인터페이스의 역할을 한다.**

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/30d72da6-d7b1-4a31-af55-7f1da26781c4/image.png)

## 34.6 사용자 정의 이터러블

### 34.6.1 사용자 정의 이터러블 구현

이터레이션 프로토콜을 준수하지 않는 일반 객체도 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다. → 피보나치 수열

```jsx
// 피보나치 수열을 구현한 사용자 정의 이터러블
const fibonacci = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수한다.
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1];
    const max = 10; // 수열의 최대값

    // next 메서드는 이터레이터 리절트 객체를 반환한다.
    return {
      next() {
        [pre, cur] = [cur, pre + cur];
        // 이터레이터 리절트 객체를 반환한다.
        return { value: cur, done: cur >= max };
      },
    };
  },
};

// 이터러블인 fibonacci 객체를 순회할 때마다 next 메서드가 호출한다.
for (const num of fibonacci) {
  console.log(num); // 1 2 3 5 8
}
```

### 34.6.4 무한 이터러블과 지연 평가

```jsx
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      [pre, cur] = [cur, pre + cur];
      // 무한을 구현해야 하므로 done 프로퍼티를 생략한다.
      return { value: cur };
    },
  };
};

// fibonacciFunc 함수는 무한 이터러블을 생성한다.
for (const num of fibonacciFunc()) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...4181 6765
}

// 배열 디스트럭처링 할당을 통해 무한 이터러블에서 3개의 요소만 취득한다.
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터를 공급한다. → 하지만 위 예제의 이터러블은 **지연 평가**를 통해 데이터를 생성한다.

지연 평가는 데이터가 필요한 시점 이전까지는 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법이다. 즉, 평가 결과가 필요할 때까지 평가를 늦추는 기법이 지연 평가다.

이처럼 지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 빠른 실행 속도를 기대할 수 있고 불필요한 메모리를 소비하지 않으며 무한도 표현할 수 있다는 장점이 있다.
