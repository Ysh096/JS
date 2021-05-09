# JavaScript 정리3

[TOC]

## 1. AJAX

- Asynchronous Javascript And XML (비동기식 js와 xml 데이터 타입)
- **서버와 통신**하기 위해 XMLHttpRequest 객체를 활용
  - 비동기식: 페이지 전체를 reload 하지 않고서도 수행됨 - 사용자의 event가 있으면 전체 페이지가 아닌 일부분만 업데이트
  - XMLHttpRequest는 동기식과 비동기식 통신 모두 지원

- XML은 이제 JSON에게 밀려서 잘 쓰이지 않는다. 이름에 XML이 들어가지만 JSON을 쓰는 기술도 이렇게 부른다.



## 2. AJAX 배경

- 2005년 Google Maps와 Gmail에 활용되는 기술을 설명하기 위해 AJAX라는 용어를 최초로 사용
- AJAX는 기존의 여러 기술을 사용하는 새로운 접근법을 설명하는 용어!

- Google Maps
  - 스크롤하는 행위(확대, 축소, 위치 이동 등) 하나하나가 모두 요청이지만 페이지는 갱신되지 않음
- Gmail
  - 메일의 전송 버튼을 눌러 놓으면 다른 페이지로 넘어가지 않고 그냥 전송된다.



## 3. XMLHttpRequest object

- XMLHttpRequest object는 서버와 상호작용하기 위해 사용되며, **전체 페이지의 새로고침 없이 URL로부터 데이터를 받아올 수 있음**

- XML 뿐만 아니라 모든 종류의 데이터를 받아오는데 사용 가능

- XMLHttpRequest()

  ```js
  const request = new XMLHttpRequest()
  const URL = 'https://jsonplaceholder.typicode.com/todos/1/'
  
  request.open('GET', URL) // 요청을 보낼 준비를 함
  request.send() // 요청을 보냄
  
  const todo = request.response // 응답을 받음
  console.log(todo) // 출력
  ```

  

## 4. Asynchronous JS

### 4.1 동기(Synchronous)와 비동기(Asynchronous)

- 동기식
  - 순차적, 직렬적 태스크 수행
  - 요청을 보낸 후 응답을 받아야만 다음 동작이 이루어짐(blocking, 코드가 막혀 있다, 위 코드가 완료되어야 다음 코드로 넘어간다)

- 비동기식
  - 병렬적 태스크 수행
  - 요청을 보낸 후 응답을 기다리지 않고 다음 동작이 이루어짐 (non-blocking)
    - 요청을 보내놓고 다음 태스크로 진행



### 4.2 비동기 사용의 이유?

- 사용자 경험
  - 지도처럼, 데이터의 크기가 굉장히 크다면 데이터를 모두 로드 한 뒤에야 앱이 실행되어 사용자는 로딩을 기다려야 하고, 앱이 멈춘 것처럼 느낀다.
  - 때문에 **많은 웹 API 기능은 현재 비동기 코드를 사용**!

- 동기식 예시

  - 버튼 클릭 후 alert 메시지의 확인 버튼을 누를 때까지 alert 이후의 코드는 실행되지 않음
  - 왜? => JS는 스레드가 하나기 때문이다.

  ```html
  <button>버튼</button>
  
  <script>
  	const btn = document.querySelector('button')
      
      btn.addEventListener('click', function () {
          alert('You clicked me!')
          const pElem = document.createElement('p')
          pElem.innerText = 'sample text'
          document.body.appendChild(pElem)
      })
  </script>
  ```

  ```python
  # 파이썬 예시
  from time import sleep
  import requests
  
  # 1. sleep
  def sleep_3_seconds():
      sleep(3)
      print('잘잤따!!!')
  
  print('이제 자야지')
  sleep_3_seconds() # 3초 기다리고 잘잤다!!!
  print('학교가자!!!') # 함수가 끝나야 실행되는 구문
  
  # 2. requests
  response = requests.get('https://jsonplaceholder.typicode.com/todos/1/')
  todo = response.json() # 요청한 거 받아서 parsing
  
  print(todo)
  ```

  

- 비동기식 예시

  - 요청을 보내고 응답을 기다리지 않고 다음 코드가 실행됨
  - 변수 todo에는 응답 데이터가 할당되지 않고 빈 문자열이 출력
  - 왜 기다려주지 않는가? => JS는 single threaded기 때문

  ```html
  <script>
  	const request = new XMLHttpRequest()
  	const URL = 'https://jsonplaceholder.typicode.com/todos/1/'
  	request.open('GET', URL)
      request.send()
      
      const todo = request.response // 응답이 오기 전에 다음 코드로 넘어간다.
      console.log(todo)
      
      ...
      
  </script>
  ```
  
  ```html
    <script>
      // 1. setTimeout
      const sleep3Seconds = function () {
        console.log('잘잤다!!')
      }
      console.log('이제 자야지')
      setTimeout(sleep3Seconds, 3000)
      console.log('학교 간다!!!')
  
      // 2. XMLHttpRequest
      const URL = 'https://jsonplaceholder.typicode.com/todos/1/'
      const xhr = new XMLHttpRequest() // 인스턴스 생성
      
      xhr.open('GET', URL) // 요청을 보낼 준비
      xhr.send() // 요청
  
      const todo = xhr.response
      console.log(todo) // '' 빈 문자열 출력
      
      xhr.onload = function () {
        if (xhr.status === 200) {
          const todo = xhr.response
          console.log(todo)
        }
      }
  
    </script>
  ```
  
  



### 4.3 Threads

- 프로그램이 작업을 완료하는데 사용할 수 있는 단일 프로세스
- 각 thread는 한번에 하나의 작업만 수행 가능
- 다음 작업을 시작하려면 반드시 앞의 작업이 완료되어야 함
- 컴퓨터의 CPU는 코어가 많아 한번에 여러 가지 일을 처리할 수 있다.



**JS는 single threaded !**

- 컴퓨터가 여러 개의 CPU를 가지고 있어도 main thread라 불리는 단일 스레드에서만 작업을 수행한다. 

- 이벤트를 처리하는 Call Stack이 하나인 언어라는 의미

- 이를 해결하기 위해 즉시 처리하지 못하는 이벤트들을 다른 곳(Web API)으로 보내서 처리하도록 하고, 처리된 이벤트들을 Task queue에 줄을 세워 놓고 Call Stack이 비면 담당자(Event Loop)가 대기 줄에서 가장 오래 된(queue의 앞) 이벤트를 Call Stack으로 보냄



### 4.4 Concurency model

- Event loop를 기반으로 하는 동시성 모델

1. Call Stack
   - 요청이 들어올 때마다 해당 요청을 순차적으로 처리하는 Stack(LIFO) 형태의 자료 구조
2. Web API(Browser API) - 바로 처리할 수 없는 것들을 여기에서 처리
   - JavaScript 엔진이 아닌 브라우저 영역에서 제공하는 API
   - setTimeout(), DOM events 그리고 AJAX로 데이터를 가져오는 **시간이 소요되는 일**들을 처리
3. Task Queue (Event Queue, Message Queue)
   - 콜백 함수가 대기하는 Queue(FIFO) 형태의 자료 구조
   - main thread가 끝난 후 실행(메인 스택이 비어야 실행)되어 후속 JavaScript 코드가 차단되는 것을 방지
4. Event Loop
   - **Call Stack이 비어있는지 확인**
   - 비어 있으면 Task Queue에서 실행 대기중인 콜백이 있는지 확인, Call Stack으로 push

```js
console.log('HI')

setTimeout(function ssafy () {
    console.log('SSAFY')
}, 3000) // 3초 후에 Task Queue에 들어가겠다는 의미! 3초 후에 실행이 된다는 의미는 아님

console.log('Bye')
```

파이썬이었다면, HI 출력 - 3초 기다림 - SSAFY 출력 - Bye 출력

JS에서는 HI를 출력하고 setTimeout 부분이 Call Stack에 들어가면, 바로 처리가 되지 않으므로 Web API에 콜백 함수를 넘긴다. 그 후 다음 코드로 넘어가서 Call Stack에 console.log('Bye')를 넣어준다. 결국 Bye가 출력되고, Web API에서 처리가 끝나면 Task Queue에 들어간 후 Call Stack이 비어있는지 Event Loop에서 확인하고 Call Stack으로 넘어간다.

HI - Bye - SSAFY



**Zero delay**

```js
console.log('HI')

setTimeout(function ssafy () {
    console.log('SSAFY')
}, 0) // 0초 뒤에 Task Queue로 가겠다! 얘도 마찬가지로 Call Stack이 비어야 실행된다.

console.log('Bye')
```

delay가 0이어도 순서에는 변함이 없음을 확인 가능



- setTimeout에서 사용한 delay는 JS가 요청을 처리하는 데 필요한 최소 시간이고, 보장된 시간은 아니다. 결국 Call Stack이 비어야 한다는 점 주의!

- 이렇게 되면 Hi SSAFY Bye라는 순차적인 처리는 불가능해 진다. 이를 해결하려면??

### 4.5 순차적인 비동기 처리하기

- Web API에 들어간 순서가 아니라 Web API에서 처리되어서 Task Queue에 들어간 순서대로 실행된다는 문제 발생!!
- 이를 해결하기 위해 순차적인 비동기 처리를 위한 2가지 작성 방식이 있다.

1. Async callbacks
   - 백그라운드에서 실행을 시작할 함수를 호출할 때 인자로 지정된 함수
   - ex) addEventListner()의 두 번째 인자
2. promise-style
   - Callback 함수의 현대적인 코드 스타일
   - XMLHttpRequest 보다 조금 더 현대적인 버전



## 5. Callback Function

day_03 확인

### 5.1 Callback Function

- 다른 함수에 인자로 전달 된 함수
- 외부 함수 내에서 호출되어 일종의 루틴 또는 작업을 완료함
- 동기식, 비동기식 모두 사용됨

```js
const btn = document.querySelector('button')

btn.addEventListener('click', function () { // 비동기 callback
    alert('completed!')
})
```

```python
numbers = [1, 2, 3]

def add_one(number):
    return number+1

print(map(add_one, numbers)) # add_one은 callback function
```

```python
from django.urls import path
from . import views

urlpattern = [
	path('', views.index), # views.index가 callback function으로 사용됨
]
```

다른 함수의 인자로 들어간 함수 = callback function



```html
<!-- 02_callback.html -->

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <button>버튼</button>

  <script>
    //1. My Custom Callback Function
    const myFunc = function (func) { //인자로 들어감, 반환 가능 => 콜백 함수
      return func
    }
    const myArgumentFunc = function () {
      return 'Hello'
    }

    const result = myFunc(myArgumentFunc)
    console.log(result)
    
    //ƒ () {
    //  return 'Hello'
    //}
    

    //2. map
    const nums = [1, 2, 3]
    const doubleNums = nums.map((num) => {
      return num * 2
    })
    console.log(doubleNums)
    //(3) [2, 4, 6]

    //3. Array Helper Method 
    // 전부 다 callback 함수를 사용한다.
    const numbers = [1, 2, 3]
    const newNumbers = []

    numbers.forEach((num) => {
      newNumbers.push(num + 1)
    })

    console.log(newNumbers)
    //(3) [2, 3, 4]

    //4. setTimeout
    const helloWorld = function () {
      console.log('happy coding!')
    }
    setTimeout(helloWorld, 2000)
    console.log('unhappy coding!')

    //5. addEventListener
    const myButton = document.querySelector('button')

    myButton.addEventListener('click', function () {
      console.log('button clicked!!')
    })

  </script>
</body>
</html>
```



### 5.2 JS의 함수는 일급 객체(First-class object)

- 일급 객체(일급 함수)
  - 다른 객체들에 적용 가능한 연산을 모두 지원하는 객체(함수)
- 일급 객체의 조건
  1. 인자로 넘길 수 있음
  2. 함수의 반환 값으로 사용할 수 있음, 함수의 리턴이 함수(파이썬도 가능)
  3. 변수에 할당할 수 있음(표현식 사용 가능)



### 5.3 왜 콜백함수를 사용하는가?

- 콜백 함수는 명시적인 호출이 아닌 **특정 루틴 혹은 action에 의해 호출**됨
- Django의 경우 '요청이 들어오면', event의 경우 '특정 이벤트가 발생하면' 이라는 조건 하에서 함수를 호출할 수 있었던 건 Callback function 메커니즘이 있기 때문에 가능

- 비동기 로직을 수행할 때 콜백 함수는 필수!!
  - 명시적인 호출이 아닌 특정 시점에 '알아서' 호출되도록 만들어야 하기 때문
  - 기다려주지 않고 언젠가 처리해야 하는 일은 콜백 함수를 활용



### 5.4 Callback Hell

콜백 함수가 또 다른 콜백 함수를 호출하고, 이것이 반복되면 연쇄 비동기 작업이 반복될 수 있다.

이를 Callback Hell 이라고 하는데, 이 경우 디버깅이 어렵고, 코드 가독성이 매우 떨어진다.

이를 해결하기 위해 새로운 문법 구조가 등장한다.



### 5.5 Promise way (Promise 방식 허용)

- 비동기 작업의 최종 완료 또는 실패를 나타내는 객체
- 성공했으면 데이터가, 없으면 에러가 들어 있음
- 성공에 대한 약속
  - .then()
- 실패에 대한 약속
  - .catch()



객체가 만들어지기 전의 세 가지 상태

1. 대기(pending): 이행하거나 거부되지 않는 초기 상태
2. 이행(fulfilled): 연산이 성공적으로 완료됨
3. 거부(rejected): 연산이 실패함



- 앞의비동기작업.then(callback)

  - 이전 작업(promise)이 성공했을 때 수행할 작업을 나타내는 콜백 함수
  - 앞의 비동기 작업이 성공 하면 callback에 적은 함수를 실행
  - 각 **콜백 함수**는 이전 작업의 **성공 결과를 인자로 전달 받음**
  - 따라서 성공했을 때의 코드를 콜백 함수 안에 작성

- 앞의비동기작업.catch(callback)

  - .then에서 하나라도 실패하면 동작 (파이썬의 try except와 유사)

  - 이전 작업의 실패로 생성된 error 객체는 catch 블록 안에서 사용할 수 있음

    ```js
    비동기.then(callback)
    	.then(callback) // 실패하면 error 발생
    	.then(callback)
    	.catch(callback()) // 에러 객체를 callback 함수에서 사용 가능
    ```



- 각각의 .then() 블록은 서로 다른 promise를 반환한다.
  - 즉 .then()을 여러 개 사용하여(chaning) 연쇄적인 작업을 수행할 수 있다.
  - 결국 여러 비동기 작업을 차례대로 수행할 수 있다!
- 마찬가지로 .then()과 .catch() 메서드는 모두 promise를 반환하기 때문에 chaning 가능

- 주의
  - 반환 값이 반드시 있어야 함
  - 없다면 콜백 함수가 이전의 promise 결과를 받을 수 없음



**Promise 기본 구조**

- 두 개의 콜백 함수를 인자로 받음
- 하나는 promise가 이행했을 때,
  - resolve()
- 다른 하나는 거부했을 때를 위함
  - reject()

```js
const myPromise = new Promise((resolve, reject) => {
    setTimeout(function () {
        resolve('성공!')
    }, 300)
}) 

myPromise
	.then(function (response) {
    console.log('Yay!' + response)
})
	.catch(function (error) {
    console.log('이런 이유로 실패했어요' + error)
})
```

- finally(callback)
  - Promise 객체를 반환
  - 결과와 상관없이 무조건 지정된 콜백 함수가 실행
  - 어떠한 인자도 전달 받지 않음
    - Promise가 성공되었는지 거절되었는지 판단할 수 없기 때문
  - 무조건 실행되어야 하는 절에서 활용
    - .then()과 .catch() 블록에서의 코드 중복을 방지



**Promise가 보장하는 특징**

1. 콜백은 자바스크립트 Event Loop가 현재 실행중인 콜 스택을 완료하기 이전에는 절대 호출되지 않음
   - Promise 콜백은 event queue에 배치되는 엄격한 순서로 호출됨
2. 비동기 작업이 성공하거나 실패한 뒤에 .then() 메서드를 이용하여 추가한 경우에도 1번과 똑같이 동작
3. .then()을 여러 번 사용하여 여러 개의 콜백을 추가할 수 있음 (Chaining)
   - 각각의 콜백은 주어진 순서대로 하나 하나 실행하게 됨
   - Chaining은 Promise의 가장 뛰어난 장점

=> web API로 넘어간 이후 함수들의 실행 순서가 뒤죽박죽(event loop에 의해 대기 시간이나 응답 등이 완료된 순서로 call stack에 할당)이 될 수 있는데, 이를 해결할 수 있다는 뜻인듯?

**Axios**

- **AJAX 요청을 하기 위한 간편한 라이브러리**
  - Asynchronous Javascript And XML 
- Promise based HTTP client for the browser and Node.js
- 브라우저를 위한 Promise 기반의 클라이언트
- 원래는 XHR 이라는 브라우저 내장 객체를 활용해 AJAX 요청을 처리하는데, 이보다 편리한 AJAX 요청이 가능하도록 도움을 줌
  - 확장 가능한 인터페이스와 함께 패키지로 사용이 간편한 라이브러리를 제공
- Promise 객체를 리턴한다. 

```js
axios.get('https://jsonplaceholder.typicode.com/todos/1/') // Promise return
	.then(..)
	.catch(..)
```

```js
const URL = 'https://jsonplaceholder.typicode.com/todos/1/'
axios.get(URL) // promise 반환
  .then(function (response) {
    console.log(response)
    return response.data
})
  .then(function (data) {
    return data.title
})
  .then(function (title) {
    console.log(title)
})
  .catch(function (error) {
    console.log(error)
})
  .finally(function () {
    console.log('이건 무조건 실행됩니다.')
})
```



axios CDN은 google에서 검색해서 github에 들어가보면 구할 수 있다.



## 6. async & await (이런게 있다! 정도) <= Vue에서 axios를 사용하기 때문에..

이제는 then 지옥에 빠진 것 같다. 이를 해결하기 위한 것



- 비동기 코드를 작성하는 새로운 방법
  - ECMAScript 2017 에서 등장
- Promise 구조의 then chaining을 제거 (Syntactic sugar)



