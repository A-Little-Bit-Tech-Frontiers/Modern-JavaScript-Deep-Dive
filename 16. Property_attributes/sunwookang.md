# 내용

## 내부 슬롯 / 내부 메서드

- 자바스크립트 엔진에서 실제로 동작하지만 개발자가 직접 접근할 수 없는 프로퍼티
- 일부 내부 슬롯과 내부 메서드에 한하여 간접적으로 접근 가능
- (슬롯은 [[]]로 감싸져 있음)

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가짐. __proto__를 통해 간접적으로 접근 가능

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/ad81082c-4770-434b-ae84-af5a213499c2/Untitled.png)

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.

- 프로퍼티 상태: 프로퍼티 값, 값의 갱신 가능 여부, 열거 가능 여부, 재정의 가능 여부
- 프로퍼티 어트리뷰트: 내부 슬롯 Value, Writable Enumerable, Configurable

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/0015154f-1fbf-4159-9701-07cde4095df5/Untitled.png)

- Object.getOwnPropertyDescriptor
    - 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체 반환

## 데이터 프로퍼티 / 접근자 프로퍼티

데이터 프로퍼티

- 키와 값으로 구성된 일반적인 프로퍼티
- Value, Writable, Enumerable, Configurable 프로퍼티 어트리뷰트를 가짐. 자동 정의된다.

접근자 프로퍼티

- 자체적으로 값을 갖지 않고 다른 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티
- Get, Set, Enumerable, Configurable 프로퍼티 어트리뷰트를 가짐.
- getter/setter 함수로도 불림

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/074d9aef-b1a5-459a-a1b6-31605cd854d5/Untitled.png)

접근자 프로퍼티 fullName에 접근할 때 순서

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/1e96ad04-b51c-4eae-850a-24a40f9cfc01/Untitled.png)

## 프로퍼티 정의

새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것.

Object.defineProperty

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/499beca4-2c2a-46a7-8655-90a58ed5a867/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/1221e1c4-c3ac-49f9-95de-524196ba2ab8/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/16236295-3f3d-419c-a40b-f72a0987a91a/Untitled.png)

## 객체 변경 방지

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/4d3d21ec-362a-4524-a261-0bbc0ecebf91/Untitled.png)

위 메서드로 만들어진 객체에 대하여 각각 Object.isExtensible, Object.isSealed, Object.isFrozen 으로 여부 확인 가능

Object.freeze로 동결하더라도 중첩 객체까지 동결할 수 없다.

→  모든 프로퍼티에 대해 재귀적으로 Object.freeze 메서드 호출

# 추가 질문

1. 그래서 이걸 어디다 쓸까?

# 참고
