## 23. 실행 컨텍스트

## 23.2 소스코드의 평가와 실행

모든 소스코드는 실행에 앞서 평가 과정을 거치며 코드를 실행하기 위한 준비를 한다. 다시 말해, 자바스크립트 엔진은 소스코드를 2개의 과정, 즉 “소스코드의 평가”와 “소스코드의 실행”과정으로 나누어 처리한다.

소스코드 평가 과정에서는 실행 컨텍스트를 생성하고 변수, 함수 등의 선언문만 먼저 실행하여 생성된 변수나 함수 식별자를 키로 실행 컨텍스트가 관리하는 스코프(렉시컬 환경의 환경 레코드)에 등록한다.

소스코드 평가 과정이 끝나면 비로소 선언문을 제외한 소스코드가 순차적으로 실행되기 시작한다. 즉, 런타임이 시작된다. 이때 소스코드 실행에 필요한 정보, 즉 변수나 함수의 참조를 실행 컨텍스트가 관리하는 스코프에서 검색해서 취득한다. 그리고 변수 값의 변경 등 소스코드의 실행 결과는 다시 실행 컨텍스트가 관리하는 스코프에 등록된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/d05f18e3-a892-43d9-a797-28d2fcaedcbb/Untitled.png)

## 23.3 실행 컨텍스트의 역할

**1.전역 코드 평가 → 2.전역 코드 실행 → 3.함수 코드 평가 → 4.함수 코드 실행**

**실행 컨텍스트는 소스코드를 실행하는 데 필요한 환경을 제공하고 코드의 실행 결과를 실제로 관리하는 영역이다.**

좀 더 구체적으로 말해, 실행 컨텍스트는 식별자(변수, 함수, 클래스 등의 이름)를 등록하고 관리하는 스코프와 코드 실행 순서 관리를 구현한 내부 메커니즘으로, 모든 코드는 실행 컨텍스트를 통해 실행되고 관리된다.

식별자와 스코프는 실행 컨텍스트의 **렉시컬 환경(Lexical EN)**으로 관리하고 코드 실행 순서는 **실행 컨텍스트 스택**으로 관리한다.

## 23.4 실행 컨텍스트 스택

```tsx
const x = 1;

function foo() {
  const y = 2;

  function bar() {
    const z = 3;
    console.log(x + y + z);
  }
  bar();
}

foo(); // 6
```

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/f6183c77-b45b-47f7-96e3-ce2e8167e871/Untitled.png)

## 23.5 렉시컬 환경

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/1672fa8c-401a-4cef-9849-e28651fb3948/Untitled.png)

## 23.6 실행 컨텍스트의 생성과 식별자 검색 과정

```jsx
var x = 1;
const y = 2;

function foo(a) {
  var x = 3;
  const y = 4;

  function bar(b) {
    const z = 5;
    console.log(a + b + x + y + z);
  }
  bar(10);
}

foo(20); // 42
```

### 23.6.2 전역 코드 평가

1. 전역 실행 컨텍스트 생성
2. 전역 렉시컬 환경 생성
   1. 전역 환경 레코드 생성
      1. 객체 환경 레코드 생성
      2. 선언적 환경 레코드 생성
   2. this 바인딩
   3. 외부 렉시컬 환경에 대한 참조 결정

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/19f23b71-21ab-4826-8fe3-e642c4db4891/Untitled.png)

**2.a. 전역 환경 레코드 생성**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/51b3d7dd-526b-43c9-96b3-e82485409ae6/Untitled.png)

기존의 var 키워드로 선언한 전역 변수와 ES6의 let, const 키워드로 선언한 전역 변수를 구분하여 관리하기 위해 전역 스코프 역할을 하는 **전역 환경 레코드는 객체 환경 레코드와 선언적 환경 레코드로 구성되어 있다.**

객체 환경 레코드는 기존의 전역 객체가 관리하던 var 키워드로 선언한 전역 변수와 함수 선언문으로 정의한 전역 함수, 빌트인 전역 프로퍼티와 빌트인 전역 함수, 표준 빌트인 객체를 관리하고, 선언적 환경 레코드는 let, const 키워드로 선언한 전역 변수를 관리한다. 즉, 전역 환경 레코드의 객체 환경 레코드와 선언적 환경 레코드는 서로 협력하여 전역 스코프와 전역 객체(전역 변수의 전역 객체 프로퍼티화)를 관리한다.

**2.a.i. 객체 환경 레코드 생성**

전역 코드 평가 과정에서 var 키워드로 선언한 전역 변수와 함수 선언문으로 정의된 전역 함수는 전역 환경 레코드의 객체 환경 레코드에 연결된 BindingObject를 통해 전역 객체의 프로퍼티와 메서드가 된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/8b1bdd5b-4980-4ae1-b22c-c7cde7eea980/Untitled.png)

**2.a.ii. 선언적 환경 레코드 생성**

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/516842c6-88a4-4258-91f7-3456157efd0b/Untitled.png)

위 그림에서 y 변수에 바인딩되어 있는 <uninitialized>는 초기화 단계가 진행되지 않아 변수에 접근할 수 없음을 나타내기 위해 사용한 표현으로 실제로 바인딩 된 값이 아니다.

let, const 키워드로 선언한 변수도 변수 호이스팅이 발생하는 것은 변함이 없다. 단, let, const 키워드로 선언한 변수는 런타임에 컨트롤이 변수 선언문에 도달하기 전까지 일시적 사각지대에 빠지기 때문에 참조할 수 없다.

```jsx
let foo = 1; // 전역 변수

{
  // let, const 키워드로 선언한 변수가 호이스팅되지 않는다면 전역 변수를 참조해야 한다.
  // 하지만 let 키워드로 선언한 변수도 여전히 호이스팅이 발생하기 때문에 참조 에러(ReferenceError)가 발생한다.
  console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
  let foo = 2; // 지역 변수
}
```

**2.2. this 바인딩**

전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에 this가 바인딩된다. 일반적으로 전역 코드에서 this는 전역 객체를 가리키므로 전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에는 전역 객체가 바인딩된다. 전역 코드에서 this를 참조하면 전역 환경 레코드의 [[GlobalThisValue]] 내부 슬롯에 바인딩되어 있는 객체가 반환된다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/a4f5d3b6-8f2f-4f92-9309-8f854020baf2/Untitled.png)

**2.3. 외부 렉시컬 환경에 대한 참조 결정**

외부 렉시컬 환경에 대한 참조(Outer Lexcical EN Reference)는 현재 평가 중인 소스코드를 포함하는 외부 소스코드의 렉시컬 환경, 즉 상위 스코프를 가리킨다. 이를 통해 단방향 링크드 리스트인 스코프 체인을 구현한다.

현재 평가 중인 소스코드는 전역 코드다. 전역 코드를 포함하는 소스코드는 없으므로 전역 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 null이 할당된다. 이는 전역 렉시컬 환경이 스코프 체인의 종점에 존재함을 의미한다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/3314bbb0-45f3-4196-80a5-488ea44f749d/Untitled.png)

## 23.6.3 전역 코드 실행

동일한 이름의 식별자가 다른 스코프에 여러개 존재할 수도 있다. 따라서 어느 스코프의 식별자를 참조하면 되는지 결정할 필요가 있다. 이를 **식별자 결정**이라 한다.

식별자 결정을 위해 식별자를 검색할 때는 실행 중인 실행 컨텍스트에서 식별자를 검색하기 시작한다. 선언된 식별자는 실행 컨텍스트의 렉시컬 환경의 환경 레코드에 등록되어 있다.

→ **스코프 체인의 동작 원리**

### 23.6.4 foo 함수 코드 평가

1. 함수 실행 컨텍스트 생성
2. 함수 렉시컬 환경 생성
   1. 함수 환경 레코드 생성
   2. this 바인딩
   3. 외부 렉시컬 환경에 대한 참조 결정

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/6ac24d98-6f5c-4b5a-8a33-a792a2c03370/Untitled.png)

자바스크립트는 함수를 어디서 호출했는지가 아니라 어디에 정의했는지에 따라 상위 스코프를 결정한다고 했다. 그리고 함수 객체는 자신이 정의된 스코프, 즉 상위 스코프를 기억한다고 했다.

자바스크립트 엔진은 함수 정의를 평가하여 함수 객체를 생성할 때 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 함수의 상위 스코프를 함수 객체의 내부 슬롯 [[Environment]]에 저장한다. 함수 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 할당되는 것은 바로 함수의 상위 스코프를 가리키는 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조다. 즉, 함수 객체의 내부 슬롯 [[Environment]]가 바로 렉시컬 스코프를 구현하는 메커니즘이다.

함수 객체의 내부 슬롯 [[Environment]]와 렉시컬 스코프는 클로저를 이해할 수 있는 중요한 단서다.

### 23.6.6 bar 함수 코드 평가

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/f9d329da-5241-475d-b80b-bd9992b84066/Untitled.png)

### 23.6.7 bar 함수 코드 실행

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/ae2c5579-9346-44ae-ba9a-9a59297ee174/Untitled.png)

실행 중인 실행 컨텍스트는 bar 함수 실행 컨텍스트다. 따라서 bar 함수 실행 컨텍스트의 bar 함수 렉시컬 환경에서 console 식별자를 검색하기 시작한다. 이곳에는 console 식별자가 없으므로 스코프 체인 상의 상위 스코프, 즉 외부 렉시컬 환경에 대한 참조가 가리키는 foo 함수 렉시컬 환경으로 이동하여 console 식별자를 검색한다.

이곳에도 console 식별자가 없으므로 스코프 체인 상의 상위 스코프, 즉 외부 렉시컬 환경에 대한 참조가 가리키는 전역 렉시컬 환경으로 이동하여 console 식별자를 검색한다.

## 23.7 실행 컨텍스트와 블록 레벨 스코프

```tsx
let x = 1;

if (true) {
  let x = 10;
  console.log(x); // 10
}

console.log(x); // 1
```

if 문의 코드 블록 내에서 let 키워드로 변수가 선언되었다. 따라서 if 문의 코드 블록이 실행되면 if 문의 코드 블록을 위한 블록 레벨 스코프를 생성해야 한다. 이를 위해 선언적 환경 레코드를 갖는 렉시컬 환경을 새롭게 생성하여 기존의 전역 렉시컬 환경을 교체한다. 이때 새롭게 생성된 if 문의 코드 블록을 위한 렉시컬 환경의 외부 렉시컬 환경에 대한 참조는 if 문이 실행되기 이전의 전역 렉시컬 환경을 가리킨다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/1607d7bf-5d7b-46ce-9594-ba956fa399c0/Untitled.png)

if 문 코드 블록의 실행이 종료되면 if 문의 코드 블록이 실행되기 이전의 렉시컬 환경으로 되돌린다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/4ff6fe7c-a1ae-4b90-836d-6b25b3d27d7b/Untitled.png)

이는 if 문뿐 아니라 블록 레벨 스코프를 생성하는 모든 블록문에 적용된다.

for 문의 변수 선언문에 let 키워드를 사용한 for 문은 코드 블록이 반복해서 실행될 때마다 코드 블록을 위한 새로운 렉시컬 환경을 생성한다. 만약 for 문의 코드 블록 내에서 정의된 함수가 있다면 이 함수의 상위 스코프는 for 문의 코드 블록이 생성한 렉시컬 환경이다.

이때 함수의 상위 스코프는 for 문의 코드 블록이 반복해서 실행될 때마다 식별자(for문의 변수 선언문 및 for문의 코드 블록 내에서 선언된 지역 변수 등)의 값을 유지해야 한다. 이를 위해 for 문의 코드 블록이 반복해서 실행될 때마다 독립적인 렉시컬 환경을 생성하여 식별자의 값을 유지한다.
