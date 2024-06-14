# 내용

자바스크립트는 객체 기반의 프로그래밍 언어

원시 값을 제외한 나머지 값은 모두 객체

0개 이상의 프로퍼티로 구성된 집합. 프로퍼티는 키와 값으로 구성

객체 생성 방법

- 클래스 기반 객체지향 언어는 클래스를 정의하고 필요한 시점에 new 연산자와 함께 생성자를 호출하여 인스턴스 생성
- 자바스크립트는 프로토타입 기반 객체지향 언어로서 다양한 방식으로 객체 생성
    - 객체 리터럴
        
        ```tsx
        const me = {
        	name: "sunwoo",
        	age: 23
        };
        ```
        
    - Object 생성자 함수
    - 생성자 함수
    - Object.create 메서드
    - 클래스

프로퍼티

- 객체는 프로퍼티의 집합
- 프로퍼티는 키와 값으로 구성
    - 키: 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
    - 값: 사용할 수 있는 모든 값
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어쓴다

프로퍼티 값

- 이미 존재하는 프로퍼티에 값을 할당하면 값이 갱신된다
    - me.age = 20
- 존재하지 않는 프로퍼티에 값을 할당하면 동적으로 추가된다
    - me.company = ‘bdacs’
- delete 연산자를 통해 해당 프로퍼티를 삭제한다
    - delete me.company

메서드

- 객체에서 값을 함수로 넣은 것이 메서드

```tsx
const game = {
	name: "pokerogue",
	start: () => console.log('Start!'),
	end() { console.log('End'); }
}
```

- 위 객체에서 start와 end는 다르게 동작한다

# 질문

1. 자바스크립트에서 객체란?
2. 프로퍼티와 메서드의 차이

# 추가 질문

1. 객체의 키는 문자열 또는 심벌만 가능. 그 외 타입의 키를 넣고 싶다면?
    - 답
        - Map
            - key로 모든 값을 허용
        - WeakMap
            - key가 객체여야함
        - 문자열로 변환하여 사용
        - Proxy
            
            ```tsx
            let handler = {
                get: function(target, name) {
                    return name in target ? target[name] : `No such property: ${name}`;
                },
                set: function(target, name, value) {
                    target[name] = value;
                    return true;
                }
            };
            
            let proxy = new Proxy({}, handler);
            
            // 객체를 키처럼 사용
            let objKey = { name: "Alice" };
            proxy[objKey] = "value associated with objKey";
            
            console.log(proxy[objKey]); // "value associated with objKey"
            ```
            
2. 객체가 생성되는 과정
    - 답
        1. 메모리 할당
            1. 힙 메모리에서 필요한 공간 할당
                1. 힙 메모리: 프로그램 실행 중에 동적으로 할당되는 메모리 영역
        2. 초기화
            1. 메모리가 할당된 후 프로퍼티와 메서드를 초기화
            2. 객체의 기본 프로토타입 설정
3. 객체가 메모리에 저장되는 과정
    - 답
        - 힙 메모리에 객체 인스턴스를 생성
        - 인스턴스가 존재하는 위치를 스택 메모리에 기록
            
4. TypeScript의 object 타입과 JavaScript의 객체와의 관계
    - 답
        - object는 원시타입을 제외한 모든 타입을 나타냄
        - 모든 자바스크립트 객체는 object에 할당 가능
5. 일반 객체와 불변 객체의 차이
    - 답
        
        일반 객체
        
        - 상태가 변경될 수 있는 객체
        - const, let과는 관계 없음
        
        불변 객체
        
        - 한 번 생성되면 상태를 변경할 수 없는 객체
        - 방법
            - Object.freeze()
            - immer
         
