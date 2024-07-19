![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/6b553bfb-2185-46f1-81c6-da6df5b92058/Untitled.png)

암묵적 전역

암묵적으로 전역 객체에 x 프로퍼티를 동적 생성

또한 전역 변수처럼 사용 가능

암묵적 전역과 같은 잠재적인 오류를 막기 위해 strict mode 사용

린트

정적 분석 기능을 통해 코드를 실행하기 전에 문법적 오류, 잠재적 오류를 찾아내고 리포팅까지 해줌

strict mode가 제한하는 오류보다 더 강력

## 적용 방법

전역의 선두 또는 함수 몸체 선두에 `'use strict';` 추가

스크립트 단위나 함수 단위로 strict mode를 적용하지 말기

- 라이브러리가 non-strict mode일 경우 문제가 생길 수 있음
- 적용된 함수가 참조할 함수 쪽에 strict mode를 적용하지 않는다면 문제가 생길 수 있음
- 즉시 실행 함수로 감싼 스크립트 단위로 적용시키기

## strict mode가 발생시키는 에러

- 암묵적 전역
- delete로 변수, 함수, 매개변수 삭제
- 중복된 매개변수 이름
- with 문 사용
    - 객체를 스코프 체인에 추가하는 함수?

## strict mode 적용에 의한 변화

- 생성자 함수가 아닌 일반 함수의 this는 undefined
- 인수를 재할당해도 arguments에 반영되지 않음
