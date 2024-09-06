## 40.6 이벤트 전파

DOM 트리 상에 존재하는 DOM 요소 노드에서 발생한 이벤트는 DOM 트리를 통해 전파된다. 이를 이벤트 전파라고 한다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/caf0343b-39c6-4b16-b95f-3fbefa707b78/image.png)

ul 요소의 두 번째 자신 요소인 li 요소를 클릭하면 클릭 이벤트가 발생한다. 이때 생성된 이벤트 객체는 이벤트를 발생시킨 DOM 요소인 이벤트 타킷을 중심으로 DOM 트리를 통해 전파된다.

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/bf1ec4e6-0fed-4f24-ad18-6abe8bd7fe6d/ed98c5b8-85b5-4e71-b257-b1e5449e9aec/image.png)

- 캡처링 단계: 이벤트가 상위 요소에서 하위 요소 방향으로 전파
- 타깃 단계: 이벤트가 이벤트 타깃에 도달
- 버블링 단계: 이벤트가 하위 요소에서 상위 요소 방향으로 전파

이벤트 핸들러 어트리뷰트/프로퍼티 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다. 하지만 addEventListener 메서드 방식으로 등록한 이벤트 핸들러는 타깃 단계와 버블링 단계뿐만 아니라 캡처링 단계의 이벤트도 선별적으로 캐치할 수 있다. 캡처링 단계의 이벤트를 캐치하려면 addEventListener 메서드의 3번째 인수로 true를 전달해야 한다. 3번째 인수를 생략하거나 false를 전달하면 타깃 단계와 버블링 단계의 이벤트만 캐치할 수 있다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <ul id="fruits">
      <li id="apple">Apple</li>
      <li id="banana">Banana</li>
      <li id="orange">Orange</li>
    </ul>
    <script>
      const $fruits = document.getElementById('fruits');
      const $banana = document.getElementById('banana');

      // #fruits 요소의 하위 요소인 li 요소를 클릭한 경우 캡처링 단계의 이벤트를 캐치
      $fruits.addEventListener(
        'click', e => {
          console.log(`이벤트 단계: ${e.eventPhase}`); // 1: 캡처링 단계
          console.log(`이벤트 타깃: ${e.target}`); // [object HTMLElement]
          console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
        }, true
      );

      // 타깃 단계의 이벤트를 캐치
      $banana.addEventListener('click', e => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 2: 타깃 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLULIElement]
      });

      // 버블링 단계의 이벤트를 캐치
      $fruits.addEventListener('click', e => {
        console.log(`이벤트 단계: ${e.eventPhase}`); // 3: 버블링 단계
        console.log(`이벤트 타깃: ${e.target}`); // [object HTMLElement]
        console.log(`커런트 타깃: ${e.currentTarget}`); // [object HTMLUListElement]
      });
    </script>
  </body>
</html>
```

**이처럼 이벤트는 이벤트를 발생시킨 이벤트 타깃은 물론 상위 DOM 요소에서도 캐치할 수 있다.** 즉, DOM 트리를 통해 전파되는 이벤트는 이벤트 패스(이벤트가 통과하는 DOM 트리 상의 경로)에 위치한 모든 DOM 요소에서 캐치할 수 있다.

대부분의 이벤트는 캡처링과 버블링을 통해 전파된다. 하지만 다음 이벤트는 버블링을 통해 전파되지 않는다. 이 이벤트들은 버블링을 통해 이벤트를 전파하는지 여부를 나타내는 이벤트 객체의 공통 프로퍼티 event.bubbles의 값이 모두 false다.

- 포커스 이벤트: focus/blur
- 리소스 이벤트: load/unload/abort/error
- 마우스 이벤트: mouseenter/mouseleave

위 이벤트를 상위 요소에서 캐치해야 할 경우는 그리 많지 않지만 반드시 위 이벤트를 상위 요소에서 캐치해야 한다면 대체할 수 있는 이벤트가 존재한다.

→ 따라서 캡처링 단계에서 이벤트를 캐치해야 할 경우는 거의 없다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      html,
      body {
        height: 100%;
      }
    </style>
  </head>
  <body>
    <p>버블링과 캡처링 이벤트 <button>버튼</button></p>
    <script>
      // 버블링 단계의 이벤트를 캐치
      document.body.addEventListener('click', () => {
        console.log('Handler for body.');
      });

      // 캡처링 단계의 이밴트를 캐치
      document.querySelector('p').addEventListener(
        'click', () => {
          console.log('Handler for paragraph.');
        }, true
      );

      // 버블링 단계의 이벤트를 캐치
      document.querySelector('button').addEventListener('click', () => {
        console.log('Handler for button.');
      });
    </script>
  </body>
</html>
```

위 예제의 경우 body 요소는 버블링 단계

body, button 요소는 버블링 단계의 이벤트를 캐치하고, p 요소는 캡처링 단계의 이벤트만 캐치한다.

button 요소에서 클릭 이벤트가 발생하면 먼저 캡처링 단계를 캐치하는 p 요소의 이벤트 핸들러가 호출되고, 그후 버블링 단계의 이벤트를 캐치하는 button, body 요소의 이벤트 핸들러가 순차적으로 호출된다.

```jsx
Handler for paragraph.
Handler for button.
Handler for body.
```

만약 p 요소에서 클릭 이벤트가 발생하면 캡처링 단계를 캐치하는 p 요소의 이벤트 핸들러가 호출되고 버블링 단계를 캐치하는 body 요소의 이벤트 핸들러가 순차적으로 호출된다. 따라서 다음과 같이 출력된다.

```jsx
Handler for paragraph.
Handler for body.
```

## 40.7 이벤트 위임

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      #fruits {
        display: flex;
        list-style-type: none;
        padding: 0;
      }

      #fruits li {
        width: 100px;
        cursor: pointer;
      }

      #fruits .active {
        color: red;
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <nav>
      <ul id="fruits">
        <li id="apple" class="active">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
    </nav>
    <div>선택된 내비게이션 아이탬: <em class="msg">apple</em></div>
    <script>
      const $fruits = document.getElementById('fruits');
      const $msg = document.querySelector('.msg');

      // 사용자 클릭에 의해 선택된 내비게이션 아이탬에 active 클래스를 추가
      // 그 외에 모든 내비게이션 아이템의 active 클래스를 제거
      function activate({ target }) {
        [...$fruits.children].forEach($fruit => {
          $fruit.classList.toggle('active', $fruit === target);
          $msg.textContent = target.id;
        });
      }

      // 모든 내비게이션 아이템에 이벤트 핸들러를 등록
      document.getElementById('apple').onclick = activate;
      document.getElementById('banana').onclick = activate;
      document.getElementById('orange').onclick = activate;
    </script>
  </body>
</html>
```

이벤트 위임은 여러 개의 하위 DOM 요소에 각각 이벤트 핸들러를 등록하는 대신 하나의 상위 DOM 요소에 이벤트 핸들러를 등록하는 방법을 말한다.

→ 이벤트 위임을 통해 상위 DOM 요소에 이벤트 핸들러를 등록하면 여러 개의 하위 DOM 요소에 이벤트 핸들러를 등록할 필요가 없다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
    <style>
      #fruits {
        display: flex;
        list-style-type: none;
        padding: 0;
      }

      #fruits li {
        width: 100px;
        cursor: pointer;
      }

      #fruits .active {
        color: red;
        text-decoration: underline;
      }
    </style>
  </head>
  <body>
    <nav>
      <ul id="fruits">
        <li id="apple" class="active">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
    </nav>
    <div>선택된 내비게이션 아이탬: <em class="msg">apple</em></div>
    <script>
      const $fruits = document.getElementById('fruits');
      const $msg = document.querySelector('.msg');

      // 사용자 클릭에 의해 선택된 내비게이션 아이탬에 active 클래스를 추가
      // 그 외에 모든 내비게이션 아이템의 active 클래스를 제거
      function activate({ target }) {
        // 이벤트를 발생시킨 요소(target)가 ul#fruits의 자식 요소가 아니라면 무시
        if (!target.matches('#fruits > li')) return;

        ;[...$fruits.children].forEach($fruit => {
          $fruit.classList.toggle('active', $fruit === target);
          $msg.textContent = target.id;
        })
      }

      // 이벤트 위임: 상위 요소(ul#fruits)는 하위 요소의 이벤트를 캐치할 수 있다.
      $fruits.onclick = activate;
    </script>
  </body>
</html>
```

```jsx
$fruits.onclick = activate;
```

이벤트 객체의 currentTarget 프로퍼티는 언제나 변함없이 $fruits 요소를 가리키지만 이벤트 객체의 target 프로퍼티는 실제로 이벤트를 발생시킨 DOM 요소를 가리킨다. $fruits 요소도 클릭 이벤트를 발생시킬 수 있으므로 이 경우 이벤트 객체의 currentTarget 프로퍼티와 target 프로퍼티는 동일한 $fruits 요소를 가리키지만 $fruits 요소의 하위 요소에서 클릭 이벤트가 발생한 경우 이벤트 객체의 currentTarget 프로퍼티와 target 프로퍼티는 다른 DOM 요소를 가리킨다.

## 40.9 이벤트 핸들러 내부의 this

### 40.9.2 이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식

이벤트 핸들러 프로퍼티 방식과 addEventListener 메서드 방식 모두 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리킨다. 즉, 이벤트 핸들러 내부의 this는 이벤트 객체의 currentTarget 프로퍼티와 같다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>
    <script>
      const $button1 = document.querySelector('.btn1');
      const $button2 = document.querySelector('.btn2');

      // 이벤트 핸들러 프로퍼티 방식
      $button1.onclick = function(e) {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // $button1
        console.log(e.currentTarget); // $button1
        console.log(this === e.currentTarget); // true

        // $button1의 textContent를 1 증가.
        ++this.textContent;
      }

      // addEventListener 메서드 방식
      $button2.addEventListener('click', function(e) {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // $button2
        console.log(e.currentTarget); // $button2
        console.log(this === e.currentTarget); // true

        // $button2의 textContent를 1 증가.
        ++this.textContent;
      })
    </script>
  </body>
</html>
```

화살표 함수로 정의한 이벤트 핸들러 내부의 this는 상위 스코프의 this를 가리킨다. 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn1">0</button>
    <button class="btn2">0</button>
    <script>
      const $button1 = document.querySelector('.btn1');
      const $button2 = document.querySelector('.btn2');

      // 이벤트 핸들러 프로퍼티 방식
      $button1.onclick = e => {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // window
        console.log(e.currentTarget); // $button1
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN을 할당한다.
        ++this.textContent;
      }

      // addEventListener 메서드 방식
      $button2.addEventListener('click', e => {
        // this는 이벤트를 바인딩한 DOM 요소를 가리킨다.
        console.log(this); // window
        console.log(e.currentTarget); // $button2
        console.log(this === e.currentTarget); // false

        // this는 window를 가리키므로 window.textContent에 NaN을 할당한다.
        ++this.textContent;
      });
    </script>
  </body>
</html>
```

클래스에서 이벤트 핸들러를 바인딩하는 경우 this에 주의해야 한다. 다음 예제를 살펴보자. 다음 예제는 이벤트 핸들러 프로퍼티 방식을 사용하고 있으나 addEventListener 메서드 방식을 사용하는 경우와 동일하다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector('.btn');
          this.count = 0;

          // increase 메서드를 이벤트 핸들러로 등록
          this.$button.onclick = this.increase;
        }

        increase() {
          // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
          // this.$button은 this.$button.$button과 같다.
          this.$button.textContent = ++this.count; // TypeError
        }
      }
      new App();
    </script>
  </body>
</html>
```

위 예제의 increase 메서드 내부의 this는 클래스가 생성할 인스턴스를 가리키지 않는다. 이벤트 핸들러 내부의 this는 이벤트를 바인딩한 DOM 요소를 가리키기 때문에 increase 메서드 내부의 this는 this.$button을 가리킨다. 따라서 increase 메서드를 이벤트 핸들러로 바인딩할 때 bind 메서드를 사용해 this를 전달하여 increase 메서드 내부의 this가 클래스가 생성할 인스턴스를 가리키도록 해야 한다.

또는 클래스 필드에 할당한 화살표 함수를 이벤트 핸들러로 등록하여 이벤트 핸들러 내부의 this가 인스턴스를 가리키도록 할 수도 있다. 다만 이때 이벤트 핸들러 Increase는 프로토타입 메서드가 아닌 인스턴스 메서드가 된다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector('.btn');
          this.count = 0;

          // 화살표 함수인 increase를 이벤트 핸들러로 등록
          this.$button.onclick = this.increase;
        }

        // 클래스 필드 정의
        // increase는 인스턴스 메서드이며 내부의 this는 인스턴스를 가리킨다.
        increase = () => (this.$button.textContent = ++this.count);
      }

      new App()
    </script>
  </body>
</html>
```

## 40.11 커스텀 이벤트

이벤트가 발생하면 암묵적으로 생성되는 이벤트 객체는 발생한 이벤트의 종류에 따라 이벤트 타입이 결정된다. 하지만 Event, UIEvent, MouseEvent 같은 이벤트 생성자 함수를 호출하여 명시적으로 생성한 이벤트 객체는 임의의 이벤트 타입을 지정할 수 있다. 이처럼 개발자의 의도로 생성된 이벤트를 커스텀 이벤트라 한다.

```jsx
// KeyboardEvent 생성자 함수로 keyup 이벤트 타입의 커스텀 이벤트 객체를 생성
const keyboardEvent = new KeyboardEvent("keyup");
console.log(keyboardEvent.type); // keyup

// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent("foo");
console.log(customEvent.type); // foo
```

생성된 커스텀 이벤트 객체는 버블링되지 않으며 preventDefault 메서드로 취소할 수도 없다. 즉, 커스텀 이벤트 객체는 bubbles와 cancelable 프로퍼티의 값이 false로 기본 설정된다.

```jsx
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent("click");
console.log(customEvent.type); // click
console.log(customEvent.bubbles); // false
console.log(customEvent.cancelable); // false
```

커스텀 이벤트 객체의 bubbles또는 cancelable 프로퍼티를 true로 설정하려면 이벤트 생성자 함수의 두 번째 인수로 bubbles또는 cancelable 프로퍼티를 갖는 객체를 전달한다.

```jsx
// MouseEvent 생성자 함수로 click 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new MouseEvent("click", {
  bubbles: true,
  cancelable: true,
});

console.log(customEvent.bubbles); // true
console.log(customEvent.cancelable); // true
```

이벤트 생성자 함수로 생성한 커스텀 이벤트는 isTrusted 프로퍼티의 값이 언제나 false다. 커스텀 이벤트가 아닌 사용자의 행위에 의해 발생한 이벤트에 의해 생성된 이벤트 객체의 isTrusted 프로퍼티 값은 언제나 true다.

```jsx
// InputEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new InputEvent("foo");
console.log(customEvent.isTrusted); // false
```

### 40.11.2 커스텀 이벤트 디스패치

생성된 커스텀 이벤트는 dispatchEvent 메서드로 디스패치(이벤트를 발생시키는 행위)할 수 있다. dispatchEvent 메서드에 이벤트 객체를 인수로 전달하면서 호출하면 인수로 전달한 이벤트 타입의 이벤트가 발생한다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">Click me</button>
    <script>
      const $button = document.querySelector('.btn');

      $button.addEventListener('click', e => {
        console.log(e) // MouseEvent {...}
        alert(`${e} clicked!`);
      });

      // 커스텀 이벤트 생성
      const customEvent = new MouseEvent('click');

      // 커스텀 이벤트 디스패치(동기 처리). click 이벤트가 발생한다.
      $button.dispatchEvent(customEvent);
    </script>
  </body>
</html>
```

일반적으로 이벤트 핸들러는 비동기 처리 방식으로 동작하지만 dispatchEvent 메서드는 이벤트 핸들러를 동기 처리 방식으로 호출한다. 다시 말해, dispatch 메서드를 호출하면 커스텀 이벤트에 바인딩된 이벤트 핸들러를 직접 호출하는 것과 같다. 따라서 dispatchEvent 메서드로 이벤트를 디스패치 하기 이전에 커스텀 이벤트를 처리할 이벤트 핸들러를 등록해야 한다.

```jsx
// CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
const customEvent = new CustomEvent("foo");
console.log(customEvent.type); // foo
```

이때 CustomEvent 이벤트 생성자 함수에는 두 번째 인수로 이벤트와 함께 전달하고 싶은 정보를 담은 detail 프로퍼티를 포함하는 객체를 전달할 수 있다.

```jsx
<!DOCTYPE html>
<html lang="kr">
  <head>
    <meta charset="UTF-8" />
    <title>Practice</title>
  </head>
  <body>
    <button class="btn">Click me</button>
    <script>
      const $button = document.querySelector('.btn');

      // 버튼 요소에 foo 커스텀 이벤트 핸들러를 등록
      // 커스텀 이벤트를 디스패치하기 이전에 이벤트 핸들러를 등록해야 한다.
      $button.addEventListener('foo', e => {
        // e.detail에는 CustomEvent 함수의 두 번째 인수로 전달한 정보가 담겨 있다.
        alert(e.detail.message);
      })

      // CustomEvent 생성자 함수로 foo 이벤트 타입의 커스텀 이벤트 객체를 생성
      const customEvent = new CustomEvent('foo', {
        detail: { message: 'Hello' }, // 이벤트와 함께 전달하고 싶은 정보
      })

      // 커스텀 이벤트 디스패치
      $button.dispatchEvent(customEvent);
    </script>
  </body>
</html>
```

기존 이벤트 타입이 아닌 임의의 이벤트 타입을 지정하여 커스텀 이벤트 객체를 생성한 경우 반드시 addEventListener 메서드 방식으로 이벤트 핸들러를 등록해야 한다. 이벤트 핸들러 어트리뷰트/프로퍼티 방식을 사용할 수 없는 이유는 on + 이벤트 타입으로 이루어진 이벤트 핸들러 어트리뷰트/프로퍼티가 요소 노드에 존재하지 않기 때문이다.
