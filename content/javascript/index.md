---
emoji: 🔮
title: JavaScript Scope / this
date: '2022-01-03 19:44:00'
author: Juko
tags: gatsby
categories: javascript
---

---
 * 티스토리 블로그에서 포스팅한 내용을 옮겨왔습니다.
---


# Scope

* 'scope' 는 자바스크립트를 포함한 모든 프로그래밍 언어의 기본적인 개념입니다.
* 우리말로 번역하면 '범위' 라는 뜻을 가지고 있습니다. 즉, 변수에 접근할 수 있는 범위라고 볼 수 있습니다.
* scope는 참조 대상 식별자(identifier)를 찾아내기 위한 규칙이며, 크게 2가지로 나뉩니다.
  * Global scope (전역 스코프)
  * Local scope (지역 스코프)

### 전역 스코프
* 전역에 선언되어 있어, 어느 곳에서든지 해당 변수에 접근 할 수 있다는 의미입니다.

### 지역 스코프
* 해당 지역에서만 접근할 수 있어, 해당 지역을 벗어난 곳에서는 접근할 수 없다는 의미입니다.
* js 안에서만 좁혀보면, 함수 코드 블록이 만든 범위로, 함수 자신과 하위 함수에서만 참조 할 수 있습니다.

```javascript
var name = 'juko'; // global scope

function printNmae() {
  var anotherName = 'john'; // local scope
  console.log(anotherName);
}

console.log(anotherName); // error!
```

* 위의 예제에서, anotherName 은 printName 이라는 함수 안에 정의되어있어, 외부에서는 접근이 불가능합니다.
* 반대로, printName 함수 안에서는 'name' 이란 변수에 접근이 가능합니다.
  * 이는 name 이 전역적인 위치에 선언되어있어 global scope 를 갖기 때문입니다.

### JavaScript scope 의 특징
* js 는 다른 언어들과 다르게 함수 레벨의 스코프를 가지고 있습니다.
  * 타 언어들은 블록 단위의 스코프를 가지며, 이는 es6 이후로 let, const 를 통해 구현되었습니다.
* 일반적으로 함수의 상위 스코프가 어디인가를 파악하기 위해 2가지 패턴이 있습니다.
  * 첫번째로는 함수를 **어디서 호출하였는가**에 따라 상위 스코프를 결정합니다.
  * 두번째로는 함수를 **어디에 선언하였는가**에 따라 상위 스코프를 결정합니다. 
    * 일반적으로 첫번째를 **동적 스코프**라 부르며, 두번째는 **정적 스코프** 또는 **렉시컬 스코프**라 부릅니다.
  * 자바스크립트를 비롯한 대부분의 언어는 렉시컬 스코프를 따른다고 볼 수 있습니다. 
    * 즉, 자바스크립트의 렉시컬 스코프는 함수를 어디서 호출하는지가 아니라, **함수를 어디에 선언하였는지**에 따라 결정된다고 볼 수 있습니다. (매우중요!)
    * 여담으로, 스코프의 결정 방향은 함수를 어디에 선언하였는지에 따라 결정됩니다만...
    * this 는 아니라는 점입니다. (아래에도 적겠지만, this 는 '함수의 호출 패턴'에 의해 결정됩니다.)

----

# this
* 일반적으로 this 라는 표현은 자기 자신을 가리킵니다.
* 그러나 JavaScript 에서의 this 는 조금 특이한데요.
* this 에 바인딩되는 객체 한가지가 아니라, **해당 함수 호출 방식**에 따라 this 바인딩되는 객체가 달라집니다.
  * 즉, js 에서의 this 는 함수 호출 패턴에 따라 달라집니다.
  * 왜 그런 것인가? 에 대해서는 아래에 재밌는 포스팅이 있으니 한번 읽어보시면 좋을 것 같습니다.
  * [자바스크립트는 왜 프로토타입을 선택했을까](https://medium.com/@limsungmook/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%8A%94-%EC%99%9C-%ED%94%84%EB%A1%9C%ED%86%A0%ED%83%80%EC%9E%85%EC%9D%84-%EC%84%A0%ED%83%9D%ED%96%88%EC%9D%84%EA%B9%8C-997f985adb42)
* 크게 4가지 케이스로 나눠볼 수 있습니다.

### 함수 호출 시 this
* 이 때 this 는 전역 객체에 바인딩 됩니다.
* 브라우저라면, 전역 객체는 window 에 바인딩 된다고 볼 수 있습니다.

### 메소드 호출 시 this
* 이 때 this 는 해당 메소드를 소유한 객체를 this 로 가리킵니다.
* 그런데 여기서, this 는 해당 메소드를 소유한 객체 뿐만 아니라, 해당 메소드를 호출한 객체에 바인딩 된다는 점입니다.
* 이게 무슨 말인가 하면 아래 예제 코드를 보면 조금 더 이해하기 쉬울 겁니다.

```javascript
const person = {
  name: 'kevin',
  printName: function () {
    console.log(this.name);
  }
};

const anotherPerson = {
  name: 'mary',
};

anotherPerson.printName = person.printName;

person.printName(); // -> kevin 이 출력됨!!
anotherPerson.printName(); // -> mary 가 출력됨!!
```

* 위에서 this 는 **함수 호출 패턴** 에 의해 달라진다고 했습니다.
* 즉, this 는 printName 을 정의한 시점에서 person 이란 객체를 가리키는 것이 아니라,
  * 해당 메소드를 호출한 시점(person.printName()) 에서 결정된다고 생각하시면 됩니다.

### .call() / .apply() / .bind() 시 this
* es6 가 본격적으로 사용되기 이전, es5 에서 원하는 this 름 참조하고 싶을 경우, 2가지 방법을 사용했는데요.
* 위와 같이 call / apply 등을 사용하거나, 상위 this 를 변수에 저장해놓고 접근하거나 했습니다.
* 일반적으로 이러한 것이 많이 사용되는 부분은 이벤트 핸들러였습니다.

### ES6 Arrow function
* 화살표 함수는 다들 많이 사용하실 겁니다.
* 특이하게도, 이 안에서의 this 는 항상 상위 스코프의 this 를 상속받습니다.
* 그리고 call 등으로 this 를 바인딩할 수 없습니다.
* 자세한 것은 [화살표함수](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Functions/Arrow_functions) 를 읽어보시면 도움이 됩니다. 