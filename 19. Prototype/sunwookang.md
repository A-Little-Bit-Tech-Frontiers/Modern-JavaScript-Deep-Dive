# 내용

## 객체지향 프로그래밍

- 프로그램을 객체의 집합으로 표현

추상화: 다양한 속성 중에서 프로그램에 필요한 속성만 간추려 내어 표현

객체: 속성을 통해 여러 개의 값을 하나의 단위로 구성한 복합적인 자료구조

객체의 상태와 상태 데이터를 조작하는 동작을 하나의 단위로 묶어 생각

상태 데이터 → 프로퍼티 / 동작 → 메서드

## 상속

어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것

→ 불필요한 중복 제거. 코드 재사용

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/fe3042d6-b27f-4f21-8a06-29725c6c95d2/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/eca9216c-49e1-46fa-a548-4518c3030912/Untitled.png)

이 생성자 함수는 인스턴스를 생성할 때마다 getArea 메서드를 중복 생성하고 모든 인스턴스가 중복 소유한다.

→ 메모리 낭비, 퍼포먼스 악영향

→ 상속을 통해 불필요한 중복 제거 필요. 프로토타입을 기반으로 상속

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/4987e0dc-f0ad-47ab-882e-9ac7bffb7ed6/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/bc3fc331-4227-4d57-8753-afad5b3881cf/Untitled.png)

Circle 생성자 함수가 생성한 모든 인스턴스는 부모 객체역할인 Circle.prototype의 모든 프로퍼티와 메서드를 상속받음

## 프로토타입 객체 (프로토타입)

상속을 구현하기 위해 사용

어떤 객체의 부모 객체 역할을 하는 객체. 다른 객체에 공유 프로퍼티를 제공

상속받은 자식 객체는 부모 객체의 프로퍼티를 자유롭게 사용

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가짐 → 프로토타입의 참조

객체가 생성될 때 생성 방식에 따라 프로토타입이 결정되고 [[Prototype]]에 저장됨

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/e01d82f0-0a8c-405d-9bd9-e02215e72e30/Untitled.png)

[[Prototype]]에는 직접 접근할 수 없지만 __proto__를 통해 자신의 프로토타입, [[Prototype]]이 가리키는 프로토타입에 간접적으로 접근 가능

상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해 __proto__를 통해 접근

순환 참조가 발생할 수도.

__proto__를 코드 내에서 직접 사용하는 것은 지양

- 모든 객체가 사용할 수 있는 것이 아니기 때문
- Object.getPrototypeOf 메서드 사용하여 프로토타입의 참조 취득
- Object.setPrototypeOf 메서드 사용하여 프로토타입 교체

함수 객체만이 소유하는 prototype 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/487180b0-898c-40e1-920b-665709f60c4e/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/cd5244c3-7765-4c57-82d3-b42e45c7467b/Untitled.png)

## 리터럴 객체의 생성자 함수와 프로토타입

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/8bb20445-8632-45f6-9c8f-ace941bae833/Untitled.png)

리터럴로 생성했더라도 Object 생성자 함수를 바라봄

그러나 new.target의 확인, 프로퍼티를 추가하는 처리 등의 내용은 다름

Function 생성자 함수를 호출하여 생성한 함수는 렉시컬 스코프를 만들지 않고 클로저도 만들지 않음

그러나 모두 constructor 프로퍼티는 Function 생성자 함수

프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재

## 프로토타입 생성 시점

생성자 함수가 생성되는 시점에 더불어 생성

함수 선언문은 런타임 이전에 먼저 실행. 함수 선언문으로 정의된 생성자 함수는 먼저 평가되어 함수 객체가 됨. 이때 프로토타입도 생성

## 객체 생성 방식과 프로토타입 결정

모든 객체 생성은 추상 연산 OrdinaryObjectCreate에 의해 생성됨

프로토타입은 해당 추상 연산에 전달되는 인수에 의해 결정됨

이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정됨

객체 리터럴

- 전달되는 프로토타입: Object.prototype

Object 생서자 함수

- 전달되는 프로토타입: Object.prototype

생성자 함수

- 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체
    
    ![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/da470a0f-7c7f-4401-a712-40726bab9168/Untitled.png)
    
- me 객체(인스턴스)는 생성자 함수인 Person의 prototype에 바인딩되어 있는 객체인 Person.prototype이 전달된다.

## 프로토타입 체인

객체의 프로퍼티에 접근하려고 할 때 해당 객체에 프로퍼티가 없으면 [[Prototype]] 내부 슬롯의 참조를 따라 부모 프로토타입의 프로퍼티를 순차적으로 검색 → 프로토타입 체인

프로토타입 체인의 최상위 객체는 언제나 Object.prototype → 모든 객체는 Object.prototype을 상속받는다 → 프로토타입 체인의 종점

Object.prototype의 프로토타입은 null

프로토타입 체인을 따라 끝(Object.prototype)까지 검색해도 없는 경우 undefined를 반환

이와 반대로 식별자는 스코프 체인에서 검색

## 오버라이딩과 프로퍼티 섀도잉

프로토타입 프로퍼티: 포로토타입이 소유한 프로퍼티

인스턴스 프로퍼티: 인스턴스가 소유한 프로퍼티

인스턴스에 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 추가하면 인스턴스 프로퍼티로 추가됨

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/cc0d05f2-178f-43ec-bd2f-47868b3916ff/Untitled.png)

me 인스턴스에 sayHello라는 인스턴스 메서드가 프로토타입 메서드를 오버라이딩

프로퍼티 섀도잉: 상속 관계에 의해 인스턴스 메서드에 의해서 프로토타입 메서드가 가려지는 현상

오버라이딩: 상위 클래스가 가지고 있는 메서드를 하위 클래스가 재정의하여 사용

오버로딩: 함수 이름은 동일하지만 매개변수의 타입 또는 개수가 다른 메서드 구현. 매개변수에 의해 메서드를 구별하여 호출되는 방식 → JS에 없으나 arguments 객체를 통해 할 순 있다

프로퍼티를 삭제할 때도 인스턴스 메서드가 삭제됨

하위 객체를 통해 프로토타입의 프로퍼티를 변경 or 삭제하는 것은 불가능

직접 프로토타입에 접근해서 처리해야함

## 프로토타입의 교체

프로토타입은 임의의 다른 객체로 변경 가능 → 객체 간의 상속 관계를 동적으로 변경 가능

### 생성자 함수에 의한 교체

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/20ae9f3d-5144-4f5e-80a8-480b1136c2a5/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/741a5fe3-99ae-402f-813d-1394dba8ddb5/Untitled.png)

### 인스턴스에 의한 교체

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/fe8fdf14-1830-45ce-9932-0917285c7d81/Untitled.png)

이미 생성된 객체의 프로토타입 교체

프로토타입을 교체해 상속 관계를 동적으로 변경하는 것은 직접 상속을 통해 교체

## instanceof

객체 instanceof 생성자 함수

생성자 함수의 prototype에 바인딩된 객체가 좌변 객체의 프로토타입 체인 상에 존재하면 true

## 직접 상속

### Object.create

명시적으로 프로토타입을 지정하여 새로운 객체 생성

- new 연산자 없이 객체 생성
- 프로토타입을 지정하면서 객체 생성
- 객체 리터럴에 의해 생성된 객체도 상속 가능

### 객체 리터럴 내부에서 __proto__에 의한 직접 상속

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/5483a90c-bbd0-4f4b-90be-236cb2c7252b/Untitled.png)

## 정적 프로퍼티/메서드

생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/b3bbdd12-7a33-4b31-a3b8-c7ff3461df21/Untitled.png)

Object의 create, keys 등도 정적 메서드

정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인의 속한 객체가 아니라서 인스턴스는 접근 불가

## 프로퍼티 존재 확인

### in

객체 내에 특정 프로퍼티가 존재하는지 여부 확인

Reflect.has 메서드 사용 가능

### Object.prototype.hasOwnProperty

객체에 특정 프로퍼티가 존재하는지 확인

## 프로퍼티 열거

### for … in

객체의 모든 프로퍼티를 순회하며 열거

상속받은 프로토타입의 프로퍼티까지 열거

→ 물론 열거 가능한 프로퍼티만 → 프로퍼티 어트리뷰트 [[Enumerable]]이 true

프로토타입 체인 상에 존재하는 모든 프로퍼티를 가져옴

### Object.keys/values/entries

객체 자신의 고유 프로퍼티만 열거
