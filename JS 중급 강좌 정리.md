# JS 중급 강좌 정리

Youtube 코딩앙마 님의 영상을 정리

[TOC]

## 1. 생성자 함수

- 함수에 **new를 붙여서 실행**하면 생성자 함수가 된다.
- 생성자 함수는 **객체를 찍어내는 틀**이라고 할 수 있다.
- 관례적으로 **첫 글자를 대문자**로 쓴다.

```js
function Item(title, price){
    //this = {}
    this.title = title;
    this.price = price;
    this.showPrice = function(){
        console.log('가격은 ${price}원 입니다.');
    };
    // return this;
}

const item1 = new Item("인형", 3000);
const item2 = new Item("가방", 4000);
const item3 = new Item("지갑", 9000);

console.log(item1, item2, item3);

item3.showPrice();
```

```js
Item {title: "인형", price: 3000, showPrice: f}
Item {title: "가방", price: 4000, showPrice: f}
Item {title: "지갑", price: 9000, showPrice: f}
```

new를 붙여 함수를 실행하면 주석처리한 코드가 실행되는 것으로 보면 된다.

만약 `const item2 = Item("가방", 4000);` 처럼 수정하여 실행하면 다음과 같은 결과를 얻는다.

```js
Item {title: "인형", price: 3000, showPrice: f}
undefined // 반환하는 값이 없기 때문
Item {title: "지갑", price: 9000, showPrice: f}
```



## 2. 객체 메서드, computed property

### 2.1 Computed property

a라는 변수와 user라는 객체가 있다.

```js
let a = 'age';
const user = {
    name: 'Mike', // name: 'Mike'
    age: 30 // age: 30
}
```

이 때, age라고 쓰는 대신 [a]라고 쓰면 변수 a에 할당된 값인 age가 key로 들어가게 된다.

```js
let a = 'age';
const user = {
    name: 'Mike', // name: 'Mike'
    [a]: 30 // age: 30
}
```

식을 넣는것도 가능하다.

```js
const user = {
    [1 + 4]: 5,
    ["안녕" + "하세요"]: "Hello"
}

> {5: 5, 안녕하세요: "Hello"}
```



### 2.2 객체에서 사용할 수 있는 메서드들

1) Object.assign()

객체 복제!

```js
const user = {
    name: "Mike",
    age: 30
}

const cloneUser = user; //참조값만 복사, 하나의 객체에 두 변수가 접근

const newUser = Object.assign({}, user);
// 첫 번째 매개변수는 초기값이고, 두 번째 매개변수를 통해 들어온 값이 들어오게 된다.

{} ← {name: "Mike", age: 30}

결과↓
{
    name: "Mike",
    age: 30,
}
```

assign을 이용하면 같은 내용이지만 새로운 객체를 만들 수 있다.

**초기값을 넣어주면 병합**을 하게 된다.

```js
Object.assign({gender:"male"}, user);

=> gender: "male",
    name: "mike",
    age: 30,
```

이름이 같은 초기값이 있으면 덮어쓰게 된다.

```js
Object.assign({name:"Tom"}, user);

=>  name: "mike", // Tom은 사라짐
    age: 30,
```

**두 개 이상의 객체**를 합칠 수도 있다.

```js
const user = {
    name: "Mike"
}
const info1 = {
    age: 30,
}
const info2 = {
    gender: "male",
}
Object.assign(user, info1, info2)
// user에 info1과 info2가 합쳐짐

{
    name: "Mike",
    age: 30,
    gender: "male",
}
```



2) Object.keys()

키 배열을 반환!

```js
const user = {
    name: "Mike",
    age: 30,
    gender: "male",
}

Object.keys(user)

=> ["name", "age", "gender"]
```

3) Object.values()

value 배열을 반환!

```js
const user = {
    name: "Mike",
    age: 30,
    gender: "male",
}

Object.values(user)

=> ["Mike", 30, "male"]
```



4) Object.entries()

키/값을 묶어서 반환!

```js
const user = {
    name: "Mike",
    age: 30,
    gender: "male",
}

Object.entries(user)

=> 
[
    ["name", "Mike"],
    ["age", 30],
    ["gender", "male"],
]
```



5) Object.fromEntries()

키/값을 묶어서 넣어주면 객체로 만듦!

```js
const arr =
[
    ["name", "Mike"],
    ["age", 30],
    ["gender", "male"],
];

Object.fromEntries(arr);
=>
{
    name: "Mike",
    age: 30,
    gender: "male",
}
```



## 3. 심볼

객체의 property key는 기본적으로 문자형이다.

```js
const obj = {
    1: '1입니다',
    flase: '거짓',
}
# 문자열 배열로 반환됨
Object.keys(obj); // ["1", "false"]

# 접근시 문자열로 접근해야 함
obj['1'] // "1입니다"
obj['false'] // '거짓'
```



한 가지 key의 형태로 가능한 것이 심볼(symbol) 형이다.

```js
# 1. 심볼 만들기
const a = Symbol();
const b = Symbol();

a === b // false
a == b // false
```

심볼형은 유일성이 보장된다. 같은 값을 가져도 동등하지 않다.



```js
# 2. 설명 붙여주기
const id = Symbol('id');
const id2 = Symbol('id');

id
> Symbol(id)
id2
> Symbol(id)
결과는 같지만,
id === id2 // false
id == id2 // false
동등하지 않다.
```



심볼을 객체의 key로 사용할 수 있다.

```js
const id = Symbol('id');
const user = {
    name: "Mike",
    age: 30,
    [id]: "myid"
}

user
> {name: "Mike", age: 30, Symbol(id): "mmyid"}

user[id] // Symbol(id)가 아니라 id로 접근!
> "myid"
```



그런데 keys나 values를 사용하면 심볼이 나타나지 않는다.

```js
const id = Symbol('id');
const user = {
    name: "Mike",
    age: 30,
    [id]: "myid"
}

Object.keys(user);
> ["name", "age"]

Object.values(user);
> ["Mike", 30]

Object.entries(user);
[Array(2), Array(2)]

for(let a in user){
    console.log(a) // name, age까지만 나옴
}
```

이렇게 숨겨놓은 심볼을 어디에 사용할까?

**특정 객체의 원본 데이터는 건드리지 않고 속성을 추가할 수 있다.**

다른 사람이 만들어놓은 객체에 내가 만든 코드를 덮어써버리면 안되고, 그렇다고 해서 이름이 겹치는 것을 피하기 위해 이상한 이름의 속성을 만들어서도 안된다. 이 때 사용하는 것이 심볼이다.



```js
# Symbol의 이름(변수명이 아니라 Symbol에 들어있는 값)을 알아내는 방법
const id = Symbol('id 입니다.')
id.description; // "id 입니다."

# keyFor는 사용할 수 없다.
```

``` js
# 잘 사용하지 않는, symbol을 보기 위한 메서드
Object.getOwnPropertySymbols(user); // [Symbol(id)]  symbol만 보여주기
Reflect.ownKeys(user); // ["name", "age", Symbol(id)] Symbol을 포함한 모든 키를 보여준다.
```



**Symbol 사용 예시**

```js
// 다른 개발자가 만든 객체
const user = {
    name: "Mike",
    age: 30,
};

// 내가 작업하는 공간
user.showName = function() {}; 

// 사용자가 접속하면 보는 메세지
for (let key in user) {
    console.log(`His ${key} is ${user[key]}.`);
}

His name is Mike.
His age is 30.
His showName is function() {}. // 이런 메시지가 나온다.
```

showName이라는 키로 function() {} 을 추가해서 이런 말도 안되는 메시지가 나오게 되었다. 이런 일을 방지하기 위해, 새롭게 객체에 변수를 추가할 때 Symbol을 사용할 수 있다.

```js
// 다른 개발자가 만든 객체
const user = {
    name: "Mike",
    age: 30,
};

// 내가 작업하는 공간
const showName = Symbol('show name');
user[showName] = function() {
    console.log(this.name);
}
user[showName]();

// 사용자가 접속하면 보는 메세지
for (let key in user) {
    console.log(`His ${key} is ${user[key]}.`);
}

Mike // 내가 추가한 Symbol 메서드를 사용한 결과! 
His name is Mike.
His age is 30.
//여기에 His showName is Symbol('show name') 이런 식으로 잡히지 않음
```



### Symbol.for() <== 전역 심볼

- 하나의 심볼만 보장받을 수 있음
- 없으면 만들고, 있으면 가져오기 때문
- Symbol 함수는 매번 다른 Symbol 값을 생성하지만 Symbol.for 메서드는하나를 생성한 뒤 키를 통해 같은 Symbol을 **공유**
- 코드 어디에서든 사용할 수 있다.

```js
const id1 = Symbol.for('id');
const id2 = Symbol.for('id');

id1 === id2; // true
```

얘를 어디서 쓰냐면.. 다른 사람이 만든 객체에 새로운 속성을 추갛야 하는데, 이 속성이 내가 앞으로 만들 다른 속성과 또 겹치지 않았으면 할 때?

```js
# Symbol.for의 이름을 알아내는 방법
Symbol.keyFor(id1) // "id"
```



## 4. 숫자, 수학 method

1) toString()

- 10진수 => 2진수/16진수

- 숫자를 문자로 바꾸는데, 동시에 2진수나 16진수로 바꿀 수 있다.

```js
let num = 10;

num.toString(); // "10"
num.toString(2); // "1010"

let num2 = 255;
num2.toString(16); // "ff"
```



2) Math

```js
Math.PI; // 3.141592...

let num1 = 5.1;
let num2 = 5.7;

# 올림
Math.ceil(num1); // 6
Math.ceil(num2); // 6

# 내림
Math.floor(num1); // 5
Math.floor(num2); // 5

# 반올림
Math.round(num1); // 5
Math.round(num2); // 6

# 셋째 자리에서 반올림해서 둘째 자리까지 보여주기
let userRate = 30.1234;

Math.round(userRate * 100)/100 // 30.12, 숫자로 반환
Math.toFixed(2); // "30.12", 문자로 반환
Math.toFixed(0); // "30"
Math.toFixed(6); // "30.123400"
# 숫자로 변환하기
Number(userRate.toFixed(2)); // 30.12

# 0~1사이 무작위 숫자 생성
Math.random() // 0.26027823967117425
Math.random() // 0.63185928941158756
...

# 1~100 사이 임의의 수를 뽑고 싶다면?
Math.floor(Math.random()*100)+1 // 1은 0이 나왔을 때를 생각해서 더해준 것

0.6789 -> 67.89 -> 67 -> 68

# 최댓값
Math.max(1, 4, -1, 5, 10, 9, 5) // 10

# 최솟값
Math.min(1, 4, -1, 5, 10, 9, 5) // -1

# 절대값
Math.abs()
Math.abs(-1) // 1

# n의 m승
Math.pow(n, m)
Math.pow(2, 10) // 1024

# 제곱근
Math.sqrt()
Math.sqrt(16) // 4
```



3) isNaN()

- x는 NaN인데, x가 NaN인지 검사하는 방법은 isNaN()이 유일하다!

```js
let x = Number('x'); // NaN

x는 NaN인데, x가 NaN인지 검사하는 방법은 isNaN()이 유일하다!
x == NaN // false
x === NaN // false
NaN == NaN // false

isNaN(x) // true, x가 NaN인지 확인하는 유일한 방법
isNaN(3) // false
```



4) parseInt()

- Number는 문자가 혼용되어 있으면 동작하지 않지만, parseInt는 동작한다.

- 다만 맨 처음부터 숫자로 읽을 수 있는 부분까지만 읽고 반환한다.

- 진수 설정이 가능하다.

```js
let margin = '10px';

parseInt(margin); // 10
Number(margin); // NaN

let redColor = 'f3';
parseInt(redColor); // NaN, 문자로 시작해서 읽을 수 없음
parseInt(redColor, 16); // 243, 16진수로 읽어서 10진수로 반환

parseInt('11', 2) // 3, 2진수로 읽어서 10진수로 반환
```



5) parseFloat()

- parseInt와 같지만 부동소수점을 반환한다.

```js
let padding = '18.5%';
parseInt(padding); // 18
parseFloat(padding); // 18.5
```



## 5. 문자열 메서드

백틱은 여러 줄로 표현할 수 있다. 따옴표로 쓰려면 \n을 사용해야 함

특정 위치에 접근은 배열과 마찬가지로 할 수 있지만, 바꾸는 것은 허용되지 않는다.

```js
let desc = '안녕하세요.'
desc[2] // '하'
desc[4] = '용';
console.log(desc);
안녕하세요. // 변화 x
```



1) length

- 문자열 길이

```js
let desc = '안녕하세요.'
desc.length // 5
```



2) toUpperCase() / toLowerCase

```js
let desc = "Hi guys. Nice to meet you."

desc.toUpperCase();
"HI GUYS. NICE TO MEET YOU."

desc.toLowerCase();
"hi guys. nice to meet you."
```



3) str.indexOf(text)

```js
let desc = "Hi guys. Nice to meet you.";

desc.indexOf('to') // 14
desc.indexOf('man') // -1, 없을 때

if (desc.indexOf('Hi') > -1) { // -1보다 크면 있는 것
    console.log('Hi가 포함된 문장입니다.')
}
```



4) str.slice(n, m)

- n이상 부터 m미만 까지 슬라이싱, m이 범위를 넘어가면 끝까지만

```js
let desc = "abcdefg";

desc.slice(2) // "cdefg"
desc.slice(0, 5) // "abcde"
desc.slice(2, -2) // "cde"
```



5) str.substring(n, m)

- n부터 m까지가 아니라 n이상 m미만까지(순서 상관x), 음수는 0으로 인식

```js
let desc = "abcdefg";

desc.substring(2, 5) // "cde"
desc.substring(5, 2) // "cde"
```



6) str.substr(n, m)

- n부터 시작해서 m개를 가져옴

```js
let desc = "abcdefg"

desc.substr(2, 4) // "cdef"
desc.substr(-4, 2) // "de"
```



7) str.trim()

- 앞 뒤 공백 제거
- 뭔가 입력받을 때 사용

```js
let desc = " coding     "
desc.trim(); // "coding"
```



8) str.repeat(n)

- n번 반복

```js
let hello = "hello!"

hello.repeat(3); // "hello!hello!hello!"
```



9) 문자열 비교

```js
1 < 3 // true
"a" < "c" // true

"a".codePointAt(0); // 97 ("a"라는 문자열의 0번째 ASCII 위치)
String.fromCodePoint(97) // "a"
```



예제1)

```js
let list = [
    "01. 들어가며",
    "02. JS의 역사".
    "03. 자료형",
    "04. 함수",
    "05. 배열",
];

let newList = [];

for (let i=0; i<list.length; i++){
    newList.push(
    list[i].slice(4));
}
console.log(newList) // ["들어가며", "JS의 역사", "자료형", "함수", "배열"]
```



예제2)

```js
//금칙어: 콜라

function hasCola(str){
    if(str.indexOf('콜라')){
        console.log('금칙어가 있습니다.');
    } else{
        console.log("통과");
    }
}

hasCola("와 사이다가 짱이야!"); // 금칙어가 있습니다.
hasCola("무슨 소리, 콜라가 최고"); // 금칙어가 있습니다.
hasCola("콜라"); // 통과
```

결과가 이상하다. 맨 처음부터 살펴보자.

`hasCola("와 사이다가 짱이야!")` 를 실행하면, `indexOf`에 의해 -1이 반환되고, `if(-1)`은 `true`기 때문에 `console.log('금칙어가 있습니다.')` 가 실행된다.

마지막 예시는 `indexOf("콜라")`의 값이 0이 나와서 if(0)이 되어 else문이 실행된다.

```js
// 금칙어: 콜라, 개선!!
function hasCola(str){
    if(str.indexOf('콜라') > -1){
        console.log('금칙어가 있습니다.');
    } else{
        console.log("통과");
    }
}

hasCola("와 사이다가 짱이야!"); // 통과
hasCola("무슨 소리, 콜라가 최고"); // 금칙어가 있습니다.
hasCola("콜라"); // 금칙어가 있습니다.
```



**Include를 활용하면 더 간단!**

- 문자가 있으면 true, 없으면 false

```js
function hasCola(str){
    if(str.includes('콜라')){
        console.log('금칙어가 있습니다.');
    } else{
        console.log("통과");
    }
}

hasCola("와 사이다가 짱이야!"); // 통과
hasCola("무슨 소리, 콜라가 최고"); // 금칙어가 있습니다.
hasCola("콜라"); // 금칙어가 있습니다.



## 6. 배열 메서드

push, pop, unshift, shift까지 다뤘었음.



1) arr.splice(n, m)

- n부터 시작해서 m개를 지운다.
- 삭제된 요소는 반환된다.

```js
let arr = [1, 2, 3, 4, 5];
arr.splice(1, 2);
console.log(arr); // [1, 4, 5]
```



2) arr.splice(n, m, x)

- n부터 m까지 지우고, x 요소 추가
- 삭제된 요소는 반환된다.

```js
let arr = [1, 2, 3, 4, 5]
arr.splice(1, 3, 100, 200);
console.log(arr); // [1, 100, 200, 5]

# 두 번째 인수에 0을 넣으면 아무것도 지우지 않고 추가만 한다!
let arr = ["나는", "철수", "입니다"];
arr.splice(1, 0, "대한민국", "소방관");

console.log(arr); // ["나는", "대한민국", "소방관", "철수", "입니다"]

let arr = [1, 2, 3, 4, 5];
let result = arr.splice(1, 2);

console.log(arr); // [1, 4, 5]
console.log(result); // [2, 3]
```



3) arr.slice(n, m)

- n부터 m까지 반환
- 아무것도 쓰지 않으면 배열이 복사된다.

```js
let arr = [1, 2, 3, 4, 5]
arr.slice(1, 4); // [2, 3, 4]

let arr2 = arr.slice(); // 복사된다.
console.log(arr2);
```



4) arr.concat(arr2, arr3, ...)

- 합쳐서 새배열 반환

```js
let arr = [1, 2];
arr.concat([3, 4]); // [1, 2, 3, 4]
arr.concat([3, 4], [5, 6]); // [1, 2, 3, 4, 5, 6]
arr.concat([3, 4], 5, 6); // [1, 2, 3, 4, 5, 6]
```



5) arr.forEach(fn)

- 배열 반복

```js
let users = ["Mike", "Tom", "Jane"]

users.forEach((item, index, arr) => {
    // ..       Mike   0    users
                 Tom   1
                 Jane  2
});
```



6) arr.indexOf / arr.lastIndexOf

- 인덱스 반환, 없으면 -1 반환

```js
let arr = [1, 2, 3, 4, 5, 1, 2, 3];
arr.indexOf(3); // 2
arr.indexOf(3, 4) // 7, 3을 찾는데, 인덱스 4이상부터 찾는다.
arr.lastIndexOf(3) // 7, 끝에서부터 찾기
```



7) arr.includes()

- 포함하는지 확인

```js
let arr = [1, 2, 3];
arr.includes(2); // true
arr.includes(8); // false
```



8) arr.find(fn) / arr.findIndex(fn)

- 함수를 넣을 수 있음. 첫 번째 true 값만 반환, 없으면 undefined를 반환
- findIndex는 해당 인덱스 반환, 없으면 -1 반환

```js
let arr = [1, 2, 3, 4, 5]
# 짝수를 찾아서 반환
const result = arr.find((item)=> {
    return item % 2 === 0;
});
console.log(result); // 2 (첫 번째 짝수)

let userList = [
    {name: "Mike", age: 30},
    {name: "Jane", age: 27},
    {name: "Tom", age: 10},
]
const result = userList.find((user) => {
    if(user.age < 19){
        return true;
    }
    return false;
})

console.log(result) // Tom


const result = userList.findIndex((user) => {
    if(user.age < 19){
        return true;
    }
    return false;
})

console.log(result) // 2
```



9) arr.filter(fn)

- 조건을 만족하는 모든 요소를 반환

```js
const arr = [1, 2, 3, 4, 5, 6]
const result = arr.filter((item) => {
    return item % 2 === 0;
})

console.log(result); // [2, 4, 6]
```



10) arr.reverse()

- 역순



11) arr.map(fn)

- 함수를 받아 특정 기능을 시행하고 새로운 배열을 반환

```js
let userList = [
    {name: "Mike", age: 30},
    {name: "Jane", age: 27},
    {name: "Tom", age: 10},
]

let newUserList = userList.map(user, index) => {
    return Object.assign({}, user, {
        id: index + 1,
        isAdult: user.age > 19,
    });
});

console.log(newUserList);
console.log(userList);
```



12) join, split

```js
let arr = ["안녕", "나는", "철수야"]
let result = arr.join("-");
console.log(result); // 안녕-나는-철수야

const users = "Mike,Jane,Tom,Tony";
const result = users.split(",");

console.log(result); // ["Mike", "Jane", "Tom", "Tony"]
```



13) Array.isArray();

```js
let user = {
    name: "Mike",
    age: 30,
};

let userList = ["Mike", "Tom", "Jane"];

console.log(typeof user); // object
console.log(typeof userList); //object

console.log(Array.isArray(user)); // false
console.log(Array.isArray(userList)); // true
```

