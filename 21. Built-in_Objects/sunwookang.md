## 자바스크립트 객체의 분류

- 표준 빌트인 객체
    - 전역 객체의 프로퍼티로 제공
- 호스트 객체
    - 각 실행환경에서 추가로 제공하는 객체
    - 브라우저 환경: DOM, Canvas, fetch 등
- 사용자 정의 객체
    - 사용자가 직접 정의

## 표준 빌트인 객체

Object, String, Number, Boolean, Date, Math, JSON 등

Math, Reflect, JSON을 제외하고는 모두 생성자 함수

각 빌트인 객체가 가지고 있는 메서드들을 사용하면 편리

## 원시값과 래퍼 객체

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/97131488-82da-4165-a5d5-b3a1144f5180/54f60816-f8fa-4622-9689-9c430e6c8c01/Untitled.png)

원시값은 객체가 아닌데도 메서드를 사용 중

이는 마침표 표기법으로 접근하면 일시적으로 원시값을 연관된 객체로 변환해 주기 때문

String.prototype의 메서드를 상속받아 사용 가능

래퍼 객체

문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체

래퍼 객체 사용 후에는 원시값을 갖도록 되돌리고 가비지 컬렉션의 대상이 됨

## 전역 객체

코드가 실행되기 이전에 엔진에 의해 어떤 객체보다도 먼저 생성되는 특수 객체

브라우저 → window, Node.js → global

표준 빌트인 객체, 호스트 객체(Web API or 호스트 API), 전역 변수/함수를 프로퍼티로 가짐

빌트인 전역 프로퍼티

전역 객체의 프로퍼티

Infinity, Nan, undefined

빌트인 전역 함수

전역 객체의 메서드

eval, isFinite, isNaN, parseFloat, parseInt, encodeURI / decodeURI, encodeURIComponent / decodeURIComponent

암묵적 전역

변수 선언 없이 그냥  `y = 20;`이렇게 사용하면 전역 객체로 들어가게 된다.
