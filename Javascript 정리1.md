# Javascript 정리

[TOC]

## 브라우저에서 할 수 있는 일?

### 1. DOM(Document Object Model) 조작

- 문서(HTML) 조작을 위한 언어 독립적인 문서 모델 인터페이스
- 문서가 구조화되어 있으며 각 요소는 객체로 취급한다.
- 속성 접근 + 메서드 활용 + 프로그래밍 언어적 특성을 활용한 조작이 가능

예를 들어, 브라우저에서 개발자 도구(F12)에 들어가 console을 연 후 다음의 명령어를 입력하면 특정 결과를 보여준다.

```
window.document.title
또는
document.title
"새 탭"
```

window는 생략 가능!

내용을 바꿀 수도 있다.

```
window.document.title = 'JavaScript'
window.document.title
"JavaScript"
```



DOM 조작은 HTML 문서 한 장을 조작하는 것을 말한다.

조작 순서는 간단히 단 두 가지로 이루어진다.

1. 선택(slelect)
2. 변경(manipulation)

**DOM 관련 객체의 상속 구조**

아직 뭔지는 잘 모르겠으나 일단 알아두자.

위가 가장 최상위 단계이다.

> __EventTarget__
>
> - Event Listener를 가질 수 있는 객체가 구현하는 DOM 인터페이스
>
> __Node__
>
> - 여러 가지 DOM 타입들이 상속하는 인터페이스
>
> __Element__
>
> - Document 안의 모든 객체가 상속하는 가장 범용적인 기반 클래스
>
> - 부모인 Node와 그 부모인 EventTarget의 속성을 상속
>
> __Document__ (Element와 동일 단계)
>
> - 브라우저가 불러온 웹 페이지를 나타냄
> - DOM 트리의 진입점 역할을 수행
>
> __HTMLElement__
>
> - 모든 종류의 HTML 요소
> - 부모인 element의 속성 상속



선택과 변경 중 선택에 대해 알아보자!

### DOM 선택 관련 메서드

- Document.querySelector()

>
>
>```js
>const mainHeader = document.querySelector('h1')
>const langHeader = document.querySelector('#lang-header')
>const frameHeader = document.querySelector('#frame-header')
>```
>
>태그나 id(#id), class(.className)를 선택할 수 있다.
>
>제공한 선택자와 일치하는 element를 가장 첫번째에 있는 하나만 선택해서 **반환**한다. (없으면 null)
>
>```js
>//자손 선택자와 자식 선택자를 사용할 수 있다.
>
>// 자손 선택자(body 자손 중 li 태그가 있으면 맨 처음걸 반환)
>const selectDescendant = document.querySelector('body li')
>
>// 자식 선택자(body 직계에 li태그가 있으면 반환, 없으면 null)
>const selectChild = document.querySelector('body > li')
>```



- Document.querySelectorAll()

>
>
>```js
>const langLi = document.querySelectorAll('.lang')
>```
>
>제공한 선택자와 일치하는 여러 element를 선택
>
>**NodeList를 반환**



- getElementById()
- getElementByTagName()
- getElementByClassName()

위 세 가지는 티를 내고 싶을 때 사용하면 된다! querySelector로 모두 대체가 가능!!



NodeList가 뭘까? **반환 타입**에는 세 가지 종류가 있는데, 다음과 같다.

- 단일 element
  - getElementById()
  - querySelector()
- HTMLCollection (live collection)
  - getElementsByTagName()
  - getElementsByClassName()
  - 배열 형태로, name, id, 인덱스 속성으로 각 항목들에 접근할 수 있다.
  - live collection이란, 문서가 바뀔 때 실시간으로 업데이트되는 것을 말한다.
- NodeList (live collection)
  - **querySelectorAll() 에 의해 반환되는 NodeList만 static collection**
  - 배열 형태로, 인덱스 번호로만 각 항목들에 접근할 수 있다.
  - 단, HTMLCollection과 달리 배열에서 사용하는 foreach 함수 등 다양한 메서드를 사용할 수 있다.
  - static collection은 DOM이 변경되어도 collection 내용에 즉각적으로 영향을 주지 않는 것을 말한다.



이제 DOM의 변경 관련 메서드에 대해 알아보자!

- Document.createElement()

>```js
>const browserHeader = document.createElement('h2')
>const ul = document.createElement('ul')
>const li1 = document.createElement('li')
>const li2 = document.createElement('li')
>const li3 = document.createElement('li')
>```
>
>태그명을 이용해 HTML 요소를 만들 수 있다.



- ParentNode.append(), appendChild()

> ```js
>const body = document.querySelector('body')
> // 하나의 노드만 특정 부모의 자식 노드 리스트 중 마지막 자식으로 삽입
> body.appendChild(browserHeader)
> body.appendChild(ul)
> 
> // 여러 개의 Node 객체, DOMString을 추가할 수 있음
> ul.append(li1, li2, li3) 
> ```
> 
> body의 마지막 자식으로 browserHeader와 ul 태그를 넣는 코드. append를 이용하면 여러 개를 한 번에 넣을 수 있음.



- ChildNode.remove(), removeChild

> ```js
>// removeChild
> // ul 아래에 있는 특정 태그 요소 삭제
> ul.removeChild(li1) 
> ul.removeChild(li2)
> 
> // remove
> // 객체 자체를 삭제
> ul.remove()
> body.remove()
> // id가 content인 태그를 제거
> let el = document.queryselector('#content')
> el.remove()
> ```



- Node.innerText, innerHTML

> ```js
>// innerText
> browserHeader.innerText = 'Browsers'
> li1.innerText = 'IE'
> li2.innerText = '<strong>FireFox</strong>'
> 
> // innerHTML
> li3.innerHTML = '<strong>Chrome</strong>'
> ```
> 
> innerText를 이용해서 어떤 객체를 가져오려고 한다고 해보자. 이 때 만약 다음과 같은 내용을 가져오게 된다면,
>
> ```
><div>A</div>
> <div>B</div>
> ```
> 
> innerText는 태그 안의 A와 B만 가지고 오게 된다. raw text가 최종적으로 렌더링 된 모습을 가지고 오는 것이다. 반면에 innerHTML을 이용하여 요소를 가지고 온다면, 위의 내용 그대로를 가지고 오게 된다. HTML 마크업을 반환하게 되는 것이다.
>
> 이제 가지고 오는 상황이 아니라 처음 제시했던 것 처럼 새로운 내용을 태그에 입력하려고 한다고 해보자. 그러면 innerText의 경우에는 입력하는 내용이 무엇이든 그대로 텍스트 형태로 넣어주게 되어, 위의 예에서 li2는 `<strong>FireFox</strong>`이라는 내용을 그대로 가지게 된다. 반면에 innerHTML은 `Chrome`이라는, 태그의 기능이 적용된 결과를 li3에 넣어주게 된다.
>
> 따라서 생각해보면, innerText는 요소를 가져올 때도 텍스트만, 넣어줄 때도 텍스트의 형태로만 넣어주게 되고, innerHTML은 요소를 가져올 때도 HTML 태그 통째로 가져오고 넣어줄 때에는 HTML 태그를 텍스트 형태가 아니라 그 자체로 넣어주게 됨을 알 수 있다.
>
> innerText는 우리 눈에 보이는 그대로를 가져오고, 입력한다.
>
> innerHTML은 우리 눈에 보이지 않는 태그를 가져오고, 태그를 적용하여 입력한다.

**참고: innerHTML은 XSS(Cross-site scripting) 공격에 약하다.**



- Element.setAttribut(name, value)

>```js
>// id를 king으로 설정하기
>li3.setAttribute('id', 'king')
>```
>
>li3 요소의 id를 king으로 업데이트 혹은 추가하는 코드



- Element.getAttribute('속성이름')

> 해당 요소의 지정된 값(문자열)을 반환
>
> 인자는 값을 얻고자 하는 속성의 이름
>
> ```js
> li3_id = li3.getAttribute('id')
> => 'king'
> ```



### 2. BOM(Browser Object Model) 조작

- 자바스크립트가 브라우저와 소통하기 위한 모델
- 브라우저의 창이나 프레임을 추상화해서 프로그래밍적으로 제어할 수 있도록 제공하는 수단
  - 버튼, URL 입력창, 타이틀 바 등 브라우저 윈도우 및 웹 페이지의 일부분을 제어 가능
- window 객체는 모든 브라우저로부터 지원받으며 브라우저 window 자체를 지칭

```
// 인쇄 창
window.print()

// 탭 창
window.open()

// 확인, 취소 버튼이 있는 대화상자 창
window.confirm()
```



### 3. JavaScript Core(ECMAScript)

- Data Structure(Object, Array), Conditional Expression, Iteration

- 프로그래밍 언어





## Event

- 네트워크 활동 혹은 사용자와의 상호작용 같은 사건의 발생을 알리기 위한 객체
- 이벤트는 사용자 행동(클릭, 키보드 누르기 등)로 직접 발생하거나, **메서드 호출에 의해 만들어 질 수 있다.**

웹페이지에서 마우스를 갖다대면 토글되는 메뉴바 등은 이러한 이벤트를 통해 만들어 진 것이다.



### Event handler

- EventTarget.addEventListener()

>```html
><!-- addEventListener -->
>  <button id="my-button">나를 눌러봐!!</button>
>  <hr>
><script>
>  const myButton = document.querySelector('#my-button') // 선택
>  myButton.addEventListener('click', alertMessage) // click하면 alertMessage
></script>
>```
>
>`target.addEventListener(type, listener[, options])`
>
>위의 형식으로 작성하는데, type에 target이 반응할 이벤트의 유형을 넣고, listener에 target에 type 유형의 이벤트가 발생하면 알려줄 객체(어떤 동작을 하는 함수)를 넣는다.
>
>코드 예시는 myButton을 클릭하면 alertMessage라는 내장 함수(?)를 호출하는 예이다.
>
>한 가지 예를 더 들어보자.
>
>```html
><h2>Change My Color</h2>
>  <label for="change-color-input">원하는 색상을 영어로 입력하세요.</label>
>  <input id="change-color-input"></input>
><hr>
>
><script>
>  const colorInput = document.querySelector('#change-color-input')
>
>  const changeColor = function (event) {
>    	const h2Tag = document.querySelector('h2')
>    	h2Tag.style.color = event.target.value
>  }
>
>  colorInput.addEventListener('input', changeColor)
></script>
>```
>
>colorInput에 input 태그를 불러온 후 changeColor라는 h2 태그의 color를 event.target.value로 바꾸는 함수를 만들고, colorInput에 이벤트 리스너를 붙여주는 코드이다.
>
>이제 인풋 태그에 input 이라는 동작(제출도 아니고 그냥 입력만 하는 동작이면 됨, live로 반영)을 하면 changeColor가 실행되고, 이때 event의 target은 당연히 인풋 태그가 된다. 인풋 태그에 input 동작을 통해 적은 내용이 event.target.value이고, 따라서 여기에 적은 색깔의 이름에 따라 h2 태그가 바뀌는 모습을 볼 수 있다.



- event.preventDefault()

> 태그 등의 기본 동작이 일어나는 것을 막는 코드
>
> 예를 들어 a 태그를 클릭하면 링크로 이동이 되는데, preventDefault는 이러한 기본 동작을 막는다.
>
> ```html
> <script>
>   // 1
>   const checkBox = document.querySelector('#my-checkbox')
>   
>   checkBox.addEventListener('click', function (event) {
>     event.preventDefault()
>     console.log(event)
>   })
> </script>
> ```
>
> 위 코드는 checkBox의 기본 동작인 클릭시 체크 표시 생성 및 취소 기능을 막는 코드이다.



- event.stopPropagation()

> event가 상위로 전파되는 것을 막는 코드
>
> 예를 들어 큰 요소 안에 작은 요소가 있고, 둘 다 클릭에 반응한다고 하면, 작은 요소만 클릭해도 큰 요소 역시 클릭된 걸로 전파가 되어버린다. 이를 막기 위한 장치!



