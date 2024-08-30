## 37.1 Set

**Set 객체는 중복되지 않는 유일한 값들의 집합이다**. Set 객체는 배열과 유사하지만 다음과 같은 차이가 있다.

| **구분**            | **배열** | **Set 객체** |
| ------------------- | -------- | ------------ |
| 동일한 값 중복 포함 | O        | X            |
| 요소 순서에 의미    | O        | X            |
| 인덱스로 요소 접근  | O        | X            |

이러한 Set 객체의 특성은 수학적 집합의 특성과 일치한다. Set은 수학적 집합을 구현하기 위한 자료구조다. 따라서 Set을 통해 교집합, 합집합, 차집합, 여집합 등을 구현할 수 있다.

### 37.1.1 Set 객체의 생성

**Set 생성자 함수는 이터러블을 인수로 전달받아 Set 객체를 생성한다. 이때 이터러블의 중복된 값은 Set 객체에 요소로 저장되지 않는다.**

```jsx
const set1 = new Set([1,2,3,3]);
console.log(set1); // Set(3) {1,2,3}

conset set2 = new Set('hello');
console.log(set2); //Set(4) {'h','e','l','o'}

```

중복을 허용하지 않는 Set 객체의 특성을 활용하여 배열에서 중복된 요소를 제거할 수 있다.

```jsx
//배열의 중복 요소 제거
const uniq = (array) => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2,1,3,4]

//Set을 사용한 배열의 중복 요소 제거
const uniq = (array) => [...new Set(array)]; //**35장 스프레드 문법**을 이용하면 이터러블을 배열로 간편하게 변환할 수 있음
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2,1,3,4]
```

### 37.1.3 요소 추가

일치 비교 연산자 ===을 사용하면 NaN과 NaN을 다르다고 평가한다. 하지만 Set 객체는 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다. +0과 -0은 일치 비교 연산자 ===와 마찬가지로 같다고 평가하여 중복 추가를 허용하지 않는다.

```jsx
const set = new Set();

console.log(NaN === NaN); // false
console.log(0 === -0); //true

set.add(NaN).add(NaN);
console.log(set); // Set(1){NaN}

set.add(0).add(-0);
console.log(set); // Set(2){NaN, 0}
```

Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```jsx
const set = new Set();

set
  .add(1)
  .add("a")
  .add(true)
  .add(undefined)
  .add(null)
  .add({})
  .add([])
  .add(() => {});
console.log(set); // Set(8) {1,'a',true,undefined,null,{},[],()=>{}}
```

### 37.1.7 요소 순회

Set 객체의 요소를 순회하려면 Set.prototype.forEach 메서드를 사용한다. Set.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)을 인수로 전달한다. 이때 콜백 함수는 다음과 같이 3개의 인수를 전달받는다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 Set 객체 자체

첫 번째 인수와 두 번째 인수는 같은 값이다. 이처럼 동작하는 이유는 Array.prototype.forEach 메서드와 인터페이스를 통일하기 위함이며 다른 의미는 없다. Array.prototype.forEach 메서드의 콜백 함수는 두 번째 인수로 현재 순회 중인 요소의 인덱스를 전달받는다. 하지만 Set 객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지 않는다.

```jsx
const set = new Set([1, 2, 3]);

set.forEach((v1, v2, set) => console.log(v1, v2, set));
/*
1 1 Set(3) {1,2,3}
2 2 Set(3) {1,2,3}
3 3 Set(3) {1,2,3}
*/
```

**Set 객체는 이터러블이다.** 따라서 for…of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.

```jsx
const set = new Set([1, 2, 3]);
// Set 객체는 Set.prototype의 **33장 Symbol**.iterator 메서드를 상속받는 이터러블이다.
console.log(Symbol.iterator in set); //true

for (const value of set) {
  console.log(value); //1 2 3
}

//이터러블이기 때문에 스프레드 문법의 대상이 됨
console.log([...set]); // [1,2,3]

//이터러블이기 때문에 배열 디스트럭처링 할당의 대상이 됨
const [a, ...rest] = set;
console.log(a, rest); //1,[2,3]
```

### 37.1.8 집합 연산

**교집합**

교집합 A^B는 집합 A와 집합 B의 공통 요소로 구성된다.

```jsx
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

또는 다음과 같은 방법으로도 가능하다.

```jsx
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};
```

**합집합**

합집합 AUB는 집합 A와 집합 B의 중복 없는 모든 요소로 구성된다.

```jsx
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

또는 다음과 같은 방법으로도 가능하다.

```jsx
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);
```

**차집합**

차집합 A-B는 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성된다.

```jsx
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

또는 다음과 같은 방법으로도 가능하다.

```jsx
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);
```

**부분 집합과 상위 집합**

집합 A가 집합 B에 포함되는 경우(A≤B) 집합 A는 집합 B의 부분 집합이며, 집합 B는 집합 A의 상위 집합이다.

```jsx
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

또는 다음과 같은 방법으로도 가능하다.

```jsx
Set.prototype.isSuperset = function (subset) {
  const superSetArr = [...this];
  return [...subset].every((v) => superSetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);
```

## 37.2 Map

**Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.** Map 객체는 객체와 유사하지만 다음과 같은 차이가 있다.

| **구분**               | **객체**                | **Map 객체**          |
| ---------------------- | ----------------------- | --------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값     | 객체를 포함한 모든 값 |
| 이터러블               | X                       | O                     |
| 요소 개수 확인         | Object.keys(obj).length | map.size              |

**Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다. 이때 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.**

```jsx
const map = new Map();
console.log(map); // Map(0){}

const map1 = new Map([
  ["key1", "val1"],
  ["key2", "val2"],
  ["key1", "val1"],
]);
console.log(map1); // Map(2) {'key1' => 'val1', 'key2' => 'val2'}
```

Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다. 따라서 Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없다.

```jsx
// 단순 배열이다. 키와 값의 쌍으로 만들어줘야함 // new Map([[1,2]])
const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

### 37.2.3 요소 추가

Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이 덮어써진다. 이때 에러가 발생하지는 않는다.

일치 비교 연산자 ===을 사용하면 NaN과 NaN을 다르다고 평가한다. 하지만 Map 객체는 NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다. +0과 -0은 일치 비교 연산자 ===와 마찬가지로 같다고 평가하여 중복 추가를 허용하지 않는다.

```jsx
const map = new Map();

console.log(NaN === NaN); // false
console.log(0 === 0); // true

// NaN과 NaN을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(NaN, "value1").set(NaN, "value2");
console.log(map); // Map(1) { NaN => 'value2' }

// +0과 -0을 같다고 평가하여 중복 추가를 허용하지 않는다.
map.set(0, "value1").set(-0, "value2");
console.log(map); // Map(2) { NaN => 'value2', 0 => 'value2' }
```

### 37.2.8 요소 순회

Map 객체의 요소를 순회하려면 Map.prototype.forEach 메서드를 사용한다. Map.prototype.forEach 메서드는 Array.prototype.forEach 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체(옵션)를 인수로 전달한다. 이때 콜백 함수는 다음과 같이 3개의 인수를 전달받는다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소키
- 세 번째 인수: 현재 순회 중인 Map 객체 자체

```jsx
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

map.forEach((v, k, map) => console.log(v, k, map));
/*
developer {name: "Lee"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
designer {name: "Kim"} Map(2) {
  {name: "Lee"} => "developer",
  {name: "Kim"} => "designer"
}
*/
```

→ **Map 객체는 이터러블이다.** 따라서 for … of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상이 될 수도 있다.

Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

| **Map 메서드**        | **설명**                                                                                |
| --------------------- | --------------------------------------------------------------------------------------- |
| Map.prototype.keys    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환          |
| Map.prototype.values  | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환          |
| Map.prototype.entries | Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체 반환 |

Map 객체는 요소의 순서에 의미를 갖지 않지만 Map 객체를 순회하는 순서는 요소가 추가된 순서를 따른다.
