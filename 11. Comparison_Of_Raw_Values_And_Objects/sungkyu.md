## 11. 원시 값과 객체의 비교

- 원시 값을 변수에 할당하면 변수(확보된 메모리 공간)에는 실제 값이 저장된다. 이에 비해 객체를 변수에 할당하면 변수(확보된 메모리 공간)에는 참조 값이 저장된다.

## 11.1 원시 값

- 원시 타입의 값, 즉 원시 값은 변경 불가능한 값이다. 이에 비해 객체(참조) 타입의 값, 즉 객체는 변경 가능한 값이다.
- 변경 불가능하다는 것은 변수가 아니라 값에 대한 진술(값의 관점)이다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/d124d5c0-5e5f-4733-bbab-7d34f7944ab3/Untitled.png)

불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/2ea8711f-b9c0-48a8-81cb-7277a8fca00f/Untitled.png)

위 그림은 실제 자바스크립트 엔진의 내부 동작과 정확히 일치하지 않을 수 있으며, ECMAScript 사양에는 변수를 통해 메모리를 어떻게 관리해야 하는지 명확하게 정의되어 있지 않다. 따라서 실제 자바스크립트 엔진을 구현하는 제조사에 따라 실제 내부 동작 방식은 미묘한 차이가 있을 수 있다.

⇒ **두 변수의 원시 값은 서로 다른 메모리 공간에 저장된 별개의 값이 되어 어느 한쪽에서 재할당을 통해 변경하더라도 서로 간섭할 수 없다.**

## 11.2.1 변경 가능한 값

```jsx
var person = {
  name: "Lee",
};
```

객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 **참조 값**에 접근할 수 있다. 참조 값은 생성된 객체가 저장된 메모리 공간의 주소, 그 자체다.

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/5db164cf-4752-487c-ae55-5ebcec9217c0/Untitled.png)

객체를 할당한 변수는 재할당 없이 객체를 직접 변경할 수 있다. 즉, 재할당 없이 프로퍼티를 동적으로 추가할 수도 있고 프로퍼티 값을 갱신할 수 있으며 프로퍼티 자체를 삭제할 수도 있다.

## 11.2.2 참조에 의한 전달

객체를 가리키는 변수(원본, person)를 다른 변수(사본, copy)에 할당하면 **원본의 참조 값이 복사**되어 전달된다.(참조에 의한 전달)

![Untitled](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/8da44f9b-7cb5-467a-8459-639d54589b0a/Untitled.png)
