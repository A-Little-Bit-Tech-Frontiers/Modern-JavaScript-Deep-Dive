# 내용

객체 리터럴 외에 다양한 방법으로 객체 생성하기

## Object 생성자 함수

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/05939103-42b7-4156-83d1-9821bb3c7262/Untitled.png)

생성자 함수: new 연산자와 함께 호출하여 인스턴스를 생성하는 함수

인스턴스: 생성자 함수에 의해 생성된 객체

## 생성자 함수

### 객체 리터럴 생성 방식의 문제점

- 단 하나의 객체만 생성 → 동일한 프로퍼티의 객체를 여러 개 생성할 경우 비효율적

### 생성자 함수 생성 방식의 장점

- 객체를 생성하기 위한 탬플릿처럼 사용 → 동일한 객체 여러 개를 간편하게 생성
- 일반 함수와 동일한 방법으로 생성자 함수 정의, new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작

### 인스턴스 생성 과정

생성자 함수의 역할: 템플릿으로 동작하여 인스턴스 생성, 생성된 인스턴스를 초기화

1. 인스턴스 생성과 this 바인딩
    1. 인스턴스는 this에 바인딩됨
2. 인스턴스 초기화
    1. 생성자 함수 내부에서 코드에 의해 초기화.
3. 인스턴스 반환
    1. 모든 처리가 끝나면 this가 암묵적으로 반환
    2. 이때 this는 모든 처리가 완성된 인스턴스가 바인딩되어 있음
    3. 객체를 return한다면 해당 return이 반환됨. 원시 값이라면 무시되고 this 반환

### 내부 메서드

함수도 객체이기에 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있다.

함수 객체만을 위한 내부 슬롯

- [[Environment]], [[FormalParameters]]

함수 객체만을 위한 내부 메서드

- [[Call]] → 함수가 호출되면 이 메서드가 호출됨 → callable
- [[Construct]] → 생성자 함수로서 호출되면 이 메서드가 호출됨 → constructor

모든 함수 객체는 callable이지만 constructor일수도 non-constructor일수도 있다

### constructor, non-constructor

- constructor
    - 함수 선언문, 함수 표현식, 클래스
- non-constructor
    - 메서드, 화살표 함수

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/14af00c3-17b5-4717-8432-6a7bc0b4eb05/Untitled.png)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/e6b45293-eaa1-4acb-b580-41d98a72b845/Untitled.png)

### new.target

생성자 함수가 new 연산자 없이 호출되는 것을 방지하기 위해 사용

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/70472dc6-408f-4b57-a6cb-69f06a20a813/Untitled.png)

# 추가 질문

# 참고
