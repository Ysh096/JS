# Javascript 정리2

[TOC]

## 1. 식별자

- 변수를 구분할 수 있는 변수명
- 문자, 달러($), 밑줄(_)로 시작한다.
- 대소문자 구분, 클래스명을 제외하고는 모두 소문자로 시작
- for, if, case 등의 예약어 사용 불가능

- 식별자 작성 스타일
  - 카멜 케이스(camelCase, lower-camel-case)
    - 변수, 객체, 함수에 사용
  - 파스칼 케이스(PascalCase, upper-camel-case)
    - 클래스, 생성자에 사용
  - 대문자 스네이크 케이스(SNAKE_CASE)
    - 상수(constants)에 사용



## 2. 변수 선언

- let
  - 재할당 할 수 있는 변수 선언 시 사용
  - 변수 재선언 불가능
  - 블록 스코프
  - 선언과 할당을 따로 할 수 있음
- const
  - 재할당 할 수 없는 변수 선언 시 사용
  - 변수 재선언 불가능
  - 블록 스코프
  - 선언과 할당을 동시에 해야만 함(재할당이 불가능한 특성)
  
- var
  - 재할당, 재선언 모두 가능
  - 함수 스코프
  - 호이스팅(변수를 선언 이전에 참조하는 문제)되는 특성으로 인해 예기치 못한 문제 발생 가능

```js
let foo // 선언
console.log(foo) // undefined

foo = 11 // 할당
console.log(foo) // 11

let bar = 0 // 선언 + 할당
console.log(bar) // 0
```

---

**호이스팅이란?**

코드가 위로 끌어올려진 것 처럼 작동하는 것을 말한다. let, const, var 모두 호이스팅이 일어나는데, 이때 호이스팅은 변수를 선언하는 부분에 일어난다. 즉, 할당은 호이스팅되지 않는다.

```js
console.log(name) // undefined, 할당되지 않았기 때문
var name = "Mike"
---------------------------------------------------
    
console.log(name)
const name = "Mike" // ReferenceError

---------------------------------------------------
      
console.log(name)
let name = "Mike" // ReferenceError

```

위 예를 보면, var는 undefined가 뜨지만 나머지는 ReferenceError가 뜬다. 셋 다 선언이 호이스팅 된다면 왜 undefined가 아니라 ReferenceError가 나타나는 것일까?

그 이유는 let과 const는 TDZ(Temporal Dead Zone)의 영향을 받기 때문이다. TDZ는 할당을 하기 전에는 사용할 수 없도록 막아주는 역할을 한다.

```js
# 문제가 없는 코드
let age = 30;

function showAge(){
    console.log(age) // 30
}
showAge()
```

```js
# 문제가 발생하는 코드
let age = 30;

function showAge(){
    console.log(age) // ReferenceError
    
    let age = 20
}
showAge() 
```

위 예를 보면, 첫 번째 코드는 함수 바깥의 age를 출력하지만, 아래 코드는 ReferenceError를 출력한다. 그 이유는, 두 번째 코드의 함수 내부에서 age를 선언 및 할당하고 있기 때문에 함수 내부에서 age 선언 및 할당의 윗부분은 TDZ가 되기 때문이다. **함수 내부와 외부의 구분에 주의!**

---

**함수 스코프와 블록 스코프**

[블로그 참조](https://poiemaweb.com/es6-block-scope)

- 함수 레벨 스코프(var): 함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다. for, while 등에서 사용되는 변수는 전역 변수임.

- 블록 레벨 스코프(let, const): 모든 코드 블록(함수, if문, for문, while문, try/catch문 등) 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다.

블록이 뭔지 궁금했었는데, 어떤 특정한 기능을 하는 명령어의 내부를 말하는 것이었다. 파이썬으로 생각해보면 for문 내에서 쓰이는 변수가 블록 스코프를 따르는 것 같다. 파이썬의 while문은 해당되지 않을 수도? (밖에서 변수를 정해주고 while 문에서 변수를 조작하기도 하니까)

```js
function add() {
    //Block-level Scope
}

if(){
   //Block-level Scope
}

for(let i=0;i<10;i++){
    // Block-level Scope
}
```

```js
# var는 함수 스코프이므로 블록 외부에서 사용할 수 있음
# let, const는 이렇게 사용할 수 없음!
const age = 30;
if(age>19){
    var txt = '성인';
}
console.log(txt); // '성인'

# var가 함수 내에서 선언된 경우 외부에서 사용할 수 없음
function add(num1, num2){
    var result = num1 + num2
}
add(2, 3)
console.log(result)
# Uncaught ReferenceError: result is not defined

==> 결국 함수 내부에서 var, let, const를 선언하면 바깥에서 사용할 수 없고,
    if, for 문 등의 블록에서 선언된 let, const는 바깥에서 사용할 수 없다.
```

함수를 벗어날 수 있는 변수는 없고, var는 블록은 벗어날 수 있다. let과 const는 아무것도 벗어날 수 없다.

## 3. 타입과 연산자

### 3.1 데이터 타입 종류

- 원시 타입(Primitive type)

  - Number

    - 정수, 실수 구분 x, 부동소수점 형식을 따름
    - 계산 불가능한 경우 NaN 반환('Angel'/1004 => NaN)

  - String

    - 텍스트 데이터

    - 작은따옴표 or 큰따옴표 모두 가능

    - 16비트 유니코드 문자의 집합

    - 변수명으로 대체가 가능(템플릿 리터럴)

      ```js
      const firstName = 'Brendan'
      const lastName = 'Eich'
      const fullName = `${firstName} ${lastName}`
      ```

      backtick을 사용함에 주의!

  - Boolean

    - 논리적 참 또는 거짓을 나타내는 타입

    - true / false로 표현

    - 조건문 또는 반복문에서 유용하게 사용

    - 타입별 판별 내용

      | 데이터 타입 | 거짓       | 참               |
      | ----------- | ---------- | ---------------- |
      | Undefined   | 항상 거짓  | X                |
      | Null        | 항상 거짓  | X                |
      | Number      | 0, -0, NaN | 나머지 모든 경우 |
      | String      | 빈 문자열  | 나머지 모든 경우 |
      | Object      | X          | 항상 참          |

  - undefined
    - 변수의 값이 없음을 나타내는 데이터 타입
    - 변수 선언만 하고 할당을 하지 않으면 undefined가 할당됨
    - 원시타입이므로 typeof undefined를 해보면 undefined가 결과로 나온다.
  - null
    - **변수의 값이 없음을 의도적으로 표현**할 때 사용
    - 원시타입이지만 null을 typeof 로 검사해보면 null타입이 아니라 **object 타입**으로 나온다.
  - Symbol
  - 객체가 아닌 기본 타입들을 말하며, 변수에 해당 타입의 값이 담긴다. 다른 변수에 복사할 때 실제 값이 복사된다.

- 참조 타입(Reference type)

  - Objects
  - Array
  - Function
  - ...
  - 객체 타입의 자료형들을 말한다. 변수에 해당 객체의 **참조 값**이 담기며, 다른 변수에 복사할 때 참조 값이 복사된다.



### 3.2 연산자

- +=
- *=
- /=
- -=
- x++
- x--



- <, >, <= >=

  - 피연산자들(숫자, 문자, Boolean 등)을 비교하고 비교의 결과값을 불리언으로 반환하는 연산자

    ```js
    const numOne = 1
    const numTwo = 100
    console.log(numOne < numTwo) // true
    ```

- ==

  - 두 피연산자가 같은 값으로 평가되는지 비교 후 불리언 값을 반환
  - **암묵적 타입 변환**을 통해 타입을 일치시킨 후 같은 값인지 비교
  - 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 참조하는지 판별
  - 웬만하면 ===을 쓰자!

- ===

  - 엄격한 비교, 타입 변환 없이 타입과 값 모두 같은지 확인
  - 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 참조하는지 판별



- &&
  - and 연산
- ||
  - or 연산
- !
  - not 연산
- 단축 평가 지원
  - false && true => false
  - true || false => true



- 삼항 연산자

  - 세 개의 피연산자를 사용하여 조건에 따라 값을 반환하는 연산자

  - 가장 왼쪽의 조건식이 참이면 콜론 앞의 값을 사용, 그렇지 않으면 콜론 뒤의 값을 사용

    ```js
    console.log(true ? 1 : 2) // 1
    console.log(false ? 1 : 2) // 2
    
    const result = Math.PI > 4 ? 'Yes' : 'No'
    console.log(result) // No
    ```

    

## 4. 조건문과 반복문

### 4.1 조건문

1. **if 문**

   ```js
   const username = 'admin'
   if (username === 'admin') {
     console.log('관리자님 환영합니다.')
   } else if (username === 'manager') {
     console.log('매니저님 환영합니다.')
   } else {
     console.log(`${username}님 환영합니다.`)
   }
   ```

   

2. **switch 문**

   ```js
   const numOne = 10
   const numTwo = 100
   const operator = '+'
   
   switch (operator) {
     case '+': {
       console.log(numOne + numTwo)
       break
     }
     case '-': {
       console.log(numOne - numTwo)
       break
     }
     case '*': {
       console.log(numOne * numTwo)
       break
     }
     case '/': {
       console.log(numOne / numTwo)
       break
     }
     default: {
       console.log('유효하지 않은 연산자입니다.')
     }
   }
   ```

   표현식의 결과값과 case문의 오른쪽 값을 비교함

   break 및 default 문은 선택적으로 사용 가능

   break 문이 없는 경우 break문을 만나거나 default 문을 실행할 때 까지 다음 조건문 실행

   블록 스코프 생성



### 4.2 반복문

- while

  - 조건문이 참인 동안 반복 시행

  - 블록 스코프 생성

    ```js
    //주어진 변수 evenNumber를 2씩 증가시키고, 해당 값을 출력합니다.
    //evenNumber가 6이 되면 반복문을 종료합니다.
    
    let evenNumber = 0
    
    while (evenNumber < 6) {
      console.log(evenNumber) // 0, 2, 4
      evenNumber += 2
    }
    ```

    

- for

  - 세미콜론으로 구분되는 세 부분으로 구성
  - initialization - 최초 반복문 진입시 1회 실행되는 부분
  - condition - 매 반복 시행 전 평가되는 부분
  - expression - 매 반복 시행 이후 평가되는 부분
  - 블록 스코프 생성

  ```js
  // oddNumber라는 이름의 변수를 선언 및 1로 초기화합니다.
  // 매 시행마다 oddNumber를 2씩 증가시키고, 해당 값을 출력합니다.
  // oddNumber가 5가 되면 반복문을 종료합니다.
  
  for (let oddNumber = 1; oddNumber < 5; oddNumber += 2) {
    console.log(oddNumber) // 1, 3
  }
  ```

  

- for in

  - 객체(object)의 속성들을 순회할 때 사용
  - 배열 순회도 가능하지만 순서대로는 아닐 수 있음. 권장x
  - 실행할 코드는 중괄호 안에 작성
  - 블록 스코프 생성

  ```js
  const capitals = {
      Korea: '서울',
      France: '파리',
      USA: '워싱턴 D.C.'
  }
  for (let capital in capitals) {
      console.log(capital) // Korea, France, USA
  }
  ```

  

  ```js
  //주어진 객체를 순회하면서 예시와 같이 출력합니다.
  //예시) title: '벤자민 버튼의 시간은 거꾸로 간다'
  
  const bestMovie = {
    title: '벤자민 버튼의 시간은 거꾸로 간다',
    releaseYear: 2008,
    actors: ['브래드 피트', '케이트 블란쳇'],
    genres: ['romance', 'fantasy'],
  }
  
  for (let key in bestMovie) {
    console.log(`${key}: ${bestMovie[key]}`)
    // console.log(bestMovie[key])
  }
  ```

  

- for of

  - 반복 가능한 객체(?)를 순회하며 값을 꺼낼 때 사용 <= 객체라고는 했지만 주로 배열을 순회할 때 사용하는 듯. 위의 for in에서 사용한 객체를 for of 문으로 돌려고 하면 에러가 날 수 있다.
  - 실행할 코드는 중괄호 안에 작성
  - 블록 스코프 생성

  ```js
  const fruits = ['딸기', '바나나', '메론']
  
  for (let fruit of fruits) {
      console.log(fruit) // 딸기, 바나나, 메론
  }
  ```





## 5. 함수



### 5.1 함수 선언식(statement, declaration)

```js
function add (num1, num2) {
    return num1 + num2
}

add(2, 7)
```

함수의 이름을 선언

일반적인 프로그래밍 언어에서의 함수 설정과 같은 방법이다.

- 호이스팅이 있다. 브라우저가 자바스크립트를 해석할 때 함수 선언식을 맨 위로 끌어올려 시작하기 때문에 함수를 설정하기 이전의 코드에서도 함수를 사용할 수 있게 된다.

```js
// 함수 생성 전
logMessage()
sumNumbers()

// 함수 생성
// 1. 함수 선언식
function logMessage() {
    return 'worked'
}

// 2. 함수 표현식
const sumNumbers = function () {
    return 10+20
}
```



### 5.2 함수 표현식(expression)

```js
const sub = function (num1, num2) {
    return num1 - num2
}

sub(7, 2)
```



```js
/*
	[함수 표현식 연습]
	
	문자열로 구성된 배열을 특정 문자를 기준으로 하나의 문자열로 합치는 함수 `join`을 작성하세요.
	- 첫번째 인자 `array`는 문자열로 구성된 배열입니다.
	- 두번째 인자 `separator`는 문자열입니다.
	- 함수는 표현식으로 작성합니다.
	- 예시) join(['010', '1234', '5678'], '-')  // '010-1234-5678'
*/

const myJoin = function (array, separator) {
	let result = ''

	for (let idx = 0; idx < array.length; idx++) {
		// let word = array[idx]
		const word = array[idx]
		result += word
		if (idx < array.length - 1) {
			result += separator
		}
	}

	return result
}

console.log(myJoin(['010', '1234', '5678'], '-'))
```



기본 인자 선언 가능

```js
const greeting = function (name = 'noName') {
    console.log(`hi ${name}`)
}

greeting() // hi noName
```

- 함수 표현식을 사용하면 호이스팅시 에러가 발생한다. 왜냐하면, 함수를 변수에 할당함으로써 함수가 변수로 평가되어 변수의 scope 규칙을 따르기 때문이다.



### 5.3 Arrow Function

- function 키워드 생략 가능
- 함수의 매개변수가 단 하나 뿐이라면, '()'도 생략 가능
- 함수 바디가 표현식 하나라면 '{}' 과 return도 생략 가능

```js
const arrow = function (name) {
    return `hello! ${name}`
}

// 1. function 키워드 삭제, 중괄호를 화살표로 가리킴
const arrow = (name) => { return `hello! ${name}`}

// 2. () 생략 (함수 매개변수가 하나인 경우!)
const arrow = name => { return `hello! ${name}` }

// 3. {} & return 생략 (바디가 표현식 1개인 경우)
const arrow = name => `hello! ${name}`
```



## 6. 배열



### 6.1 배열의 정의

```js
const numbers = [1, 2, 3, 4, 5]

console.log(numbers[0]) // 1
console.log(numbers[-1]) // undefined
console.log(numbers.length) // 5
```

- 키와 속성들을 담고 있는 참조 타입의 객체(object)
- 순서를 보장하는 특징이 있음
- 대괄호로 생성, 0을 포함한 양의 정수 인덱스로 접근 가능
- 배열의 길이는 array.length로 접근 가능
- 배열의 마지막 원소는 array.length-1로 접근



### 6.2 배열 관련 메서드

1. reverse

   ```js
   const numbers = [1, 2, 3, 4, 5]
   numbers.reverse()
   console.log(numbers) // [5, 4, 3, 2, 1]
   ```

2. push & pop

   ```js
   const numbers = [1, 2, 3, 4, 5]
   
   numbers.push(100)
   console.log(numbers) // [1, 2, 3, 4, 5, 100]
   
   numbers.pop()
   console.log(numbers) // [1, 2, 3, 4, 5]
   ```

3. unshift & shift

   ```js
   const numbers = [1, 2, 3, 4, 5]
   
   numbers.unshift(100)
   console.log(numbers) // [100, 1, 2, 3, 4, 5]
   
   numbers.shift()
   console.log(numbers) // [1, 2, 3, 4, 5]
   ```

   unshift는 배열의 가장 앞에 요소를 추가하고, shift는 첫 번째 요소를 제거한다.

4. includes

   ```js
   const numbers = [1, 2, 3, 4, 5]
   
   console.log(numbers.includes(1)) // true
   
   console.log(numbers.includes(100)) // false
   ```

   배열에 특정 값이 존재하는지 판별 후 참 또는 거짓 반환

5. indexOf

   ```js
   const numbers = [1, 2, 3, 4, 5]
   let result
   
   result = numbers.indexOf(3) // 2
   console.log(result)
   
   result = numbers.indexOf(100) // -1
   console.log(result)
   ```

   배열에 특정 값이 존재하는지 확인 후 가장 첫 번째로 찾은 요소의 인덱스 반환

   만약 해당 값이 없으면 -1 반환

6. join

   ```js
   const numbers = [1, 2, 3, 4, 5]
   let result
   
   // 1. 기본 구분자(separator) == 쉼표
   result = numbers.join() // 1, 2, 3, 4, 5
   console.log(result)
   
   // 2. 구분자 지정
   result = numbers.join('') // 12345
   console.log(result)
   
   result = numbers.join(' ') // 1 2 3 4 5
   console.log(result)
   
   result = numbers.join('-') // 1-2-3-4-5
   console.log(result)
   ```

   

### 6.3 배열 관련 메서드 심화

배열을 순회하며 특정 로직을 수행하는 메서드들이 있다.

| 메서드  | 설명                                                         | 비고         |
| ------- | ------------------------------------------------------------ | ------------ |
| forEach | 배열의 각 요소에 대해 콜백 함수 한 번씩 실행                 | 반환 값 없음 |
| map     | 콜백 함수의 반환 값을 요소로 하는 새로운 배열 반환           |              |
| filter  | 콜백 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환 |              |
| reduece | 콜백 함수의 반환 값들을 하나의 값에 누적 후 반환             |              |
| find    | 콜백 함수의 반환 값이 참이면 해당 요소를 반환                |              |
| some    | 배열의 요소 중 하나라도 판별 함수를 통과하면 참을 반환       |              |
| every   | 배열의 모든 요소가 판별 함수를 통과하면 참을 반환            |              |



1. forEach(element, index, array)

   - 최대 세 개의 인자를 가질 수 있으며, 각각 element, index, array이다. 각 요소, 인덱스, 배열에 대해 원하는 동작을 설정!

   ```js
   const array = ['America', 'Japan,', 'China', 'Singapore']
   
   array.forEach((region, index, array) => {
       console.log(`${array}의 ${index}번째 요소: ${region}`)
   })
   America, Japan, China, Singapore 의 0번째 요소: America
   America, Japan, China, Singapore 의 1번째 요소: Japan
   America, Japan, China, Singapore 의 2번째 요소: China
   America, Japan, China, Singapore 의 3번째 요소: Singapore
   ```

2. map(element, index, array)

   ```js
   const numbers = [1, 2, 3, 4, 5]
   
   const doubleNums = numbers.map((num) => {
       return num * 2
   })
   
   console.log(doubleNums) // [2, 4, 6, 8, 10]
   ```

3. filter(element, index, array)

   - 함수의 반환 값이 참인 요소들만 모아서 새로운 배열을 반환

   ```js
   const numbers = [1, 2, 3]
   
   const oddNums = numbers.filter((num) => { // oddNums에 새로운 배열을 저장한다.
       return num % 2 // 홀수인 numbers의 원소만 저장한다
   })
   
   console.log(oddNums) // [1, 3, 5]
   ```

4. reduce(acc, element, index, initialValue)

   - acc의 초기값(initialValue) 지정 가능, 지정하지 않으면 배열의 첫 번째 값을 사용
   - acc는 계산할때마다 바뀐다

   ```js
   array.reduce((acc, element, index, array) => {
       // do something
   }, initialValue)
   ```

   ```js
   const numbers = [1, 2, 3]
   
   const result = numbers.reduce((acc, num) => { // result에 결과 저장
       return acc + num // 원하는 동작
   }, 0)
   
   console.log(result) // 6
   ```

5. find(element, index, array)

   ```js
   const avengers = [
       { name: 'Tony Stark', age: 45},
       { name: 'Steve Rogers', age: 32},
       { name: 'Thar', age: 40},   
   ]
   const result = avengers.find((avenger) => {
       return avenger.name === 'Tony Stark'
   })
   console.log(result) // {name: "Tony Stark", age:45}
   ```

6. some(element, index, array)

   - 요소 중 하나라도 주어진 판별 함수를 통과하면 참을 반환

   ```js
   const numbers = [1, 3, 5, 7, 9]
   
   const hasEvenNumber = numbers.sum((num) => {
       return num % 2 === 0
   })
   console.log(hasEvenNumber) // false
   ```

7. every

   - 모든 요소가 주어진 판별 함수를 통과하면 참을 반환

   ```js
   const numbers = [2, 4, 6, 8, 10]
   
   const isEveryNumberEven = numbers.every((num) => {
       return num % 2 === 0
   })
   console.log(isEveryNumberEven) // true
   ```





## 7. 객체(Objects) 중요!

### 7.1 객체의 정의

- 객체는 property의 집합이며, 중괄호 내부에 key: value 쌍으로 표현한다.
- **key는 문자열 타입만 가능**하다.
- key에 띄어쓰기 등이 있으면 따옴표로 묶어서 표현한다.
- value는 모든 타입이 가능
- 객체 요소 접근은 **점 또는 대괄호**로 가능하다.
  - **key 이름에 띄어쓰기 등의 구분자가 있으면 대괄호 접근만 가능**하다!

```js
const me = {
    name: '홍길동',
    'phone number': '01012345678',
    samsungProducts: {
        buds: 'Galaxy Buds Pro',
        galaxy: 'Galaxy s21',
    },
}

console.log(me.name) // 홍길동
consol.log(me['phone number']) // 01012345678
console.log(me.samsungProducts) // {buds: "Galaxy Buds Pro", galaxy: ...}
console.log(me.samsungProducts.buds) // Galaxy Buds Pro
```



### 7.2 객체 관련 ES6 문법

1) 속성명 축약

객체 정의시에 key와 value의 이름이 같으면 축약 가능(value를 따로 쓰지 않아도 됨)

```js
const username = 'hailey'
const contact = '010-1234-5678'

var info = {
  username,
  contact,
}
console.log(info) // {username: "hailey", contact: "010-1234-5678"}
```



2) 메서드명 축약

객체의 value로 메서드 선언 시 function 키워드 생략 가능

```js
var obj = {
    greeting: function () {
        console.log('Hi!')
    }
}
obj.greeting() // Hi!

// 축약 시
const newObj = {
    greeting() {
        console.log('Hi!')
    }
}
newObj.greeting() // Hi!
```



3) 계산된 속성 이름

객체를 정의할 때 key의 이름을 표현식을 사용하여 동적으로 생성 가능

```js
const key = 'regions'
const value = ['광주', '대전', '구미', '서울']

const countries = {
    [key]: value,
}
console.log(countries) // {regions: Array(4)}
console.log(countries.regions) // ["광주", "대전", "구미", "서울"]
```



4) 구조 분해 할당

배열 또는 객체를 분해하여 속성을 변수에 쉽게 할당할 수 있는 문법

```js
const userInformation = {
    name: 'sh',
    userId: 'skk7541',
    phoneNumber: '010-6681-4267'
}

const name = userInformation.name
const userId = userInformation.userId
const phoneNumber = userInformation.phoneNumber


// 축약
const userInformation = {
    name: 'sh',
    userId: 'skk7541',
    phoneNumber: '010-6681-4267'
}

const {name} = userInformation
const {userId} = userInformation
const {phoneNumber} = userInformation
const {email} = userInformation

// 여러 개(유용할듯?)
const {name, userId} = userInformation
```

