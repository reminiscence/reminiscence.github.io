---
emoji: 🔮
title: JavaScript Scope / this
date: '2022-01-05 20:57:00'
author: Juko
tags: js execution_context
categories: javascript
---

---
 * 티스토리 블로그에서 포스팅한 내용을 옮겨왔습니다.
---


# Execution context
* 해당 내용은 ECMAScript3 를 기준으로 하며, ES5 부터는 lexical environment 로 변경되었고, ES6 들어서도 내용이 좀 변화하였습니다.
* 그러나 해당 내용은 기본적이며, 본질은 비슷하기에, execution context 만 정리하도록 하겠습니다.
<br />
<br />
---
<br />

## 1. Execution context란?
* Execution context란(이하 EC), 쉽게 말해 JS 엔진에서 함수를 어떤 식으로 읽어들여서 수행할 것인지에 대한 정보들을 담고 있는 것이라고 할 수 있습니다.
* 예를 들어, foo 라는 함수를 호출하면, js 엔진에서는 foo 에 대한 여러 정보들을 알고 있어야 실행할 수 있습니다.
* 일반적으로 추상화된 개념이라고는 하지만, 내부적으로는 개체 형태로 정보들을 담고 있다고 합니다.
* 이러한 EC 는 수행 시 **Execution context stack** 이라는 스택에 쌓이며 내용이 처리됩니다.
  * 함수 호출 시, 함수에 대한 EC 가 생성되며, EC Stack 에 쌓이고 제어권이 넘어가게 됩니다.
  
<p style="text-align: center;"><img src="execution_context.png" /></p>
<div style="display: flex; justify-content: center;">[ Execution context stack ]</div>
<br />
<br />

## 2. Execution context의 구조

<p style="text-align: center;"><img src="execution_context_2.png" /></p>
<div style="display: flex; justify-content: center;">[ Execution context 구조 ]</div>

* Execution context는 크게 3가지 정보를 담고 있습니다.
  * 변수와 내부 함수들의 정보 (매개변수, 인수 정보 등)
  * 변수의 유효 범위 (scope)
  * this 바인딩 (this value)
* 이런 정보들은 위 그림과 같이 Variable Object, Scope chain, thisValue 를 통해 값을 담고 있습니다.
<br />
<br />

### Variable Object

<p style="text-align: center;"><img src="execution_context_3.png" /></p>
<div style="display: flex; justify-content: center;">[ Global Execution context 의 GO ]</div>

<p style="text-align: center;"><img src="execution_context_4.png" /></p>
<div style="display: flex; justify-content: center;">[ foo() Execution context - AO ]</div>

* Variable Object (이하 VO)는 변수들에 대한 정보, 내부 함수 객체에 대한 정보 등을 담고 있는 객체를 가리킵니다.
  * 직접 담고 있는게 아니라, 해당 정보를 담고 있는 객체의 주소를 가지고 있습니다. (포인터!)
* 이러한 정보를 담고 있는 객체는 전역코드를 수행할 때와 일반 함수 호출에 의해 함수에 대한 execution context를 생성했을 때 명칭이 약간 다릅니다.
* 전역 코드를 수행하려고 execution context를 생성하면, 이때 변수 정보들과 함수 객체 정보들을 담고 있는 객체를 Global Object(줄여서 GO)라고 부릅니다.
* 일반 함수를 호출했을 때에는 Activation Object(줄여서 AO)라고 부릅니다.
* GO와 AO는 거의 같으나, AO 같은 경우 함수 호출에 의해 생성되었으므로 매개변수와 arguments 정보를 추가로 담고 있습니다.
<br />
<br />

### Scope Chain

<p style="text-align: center;"><img src="execution_context_5.png" /></p>
<div style="display: flex; justify-content: center;">[ Scope chain ]</div>

* Scope chain 은 리스트 형태를 가지고 있으며, 자신과 자신의 상위에 대한 VO의 주소를 담고 있습니다.
* 리스트의 제일 마지막은 전역 객체인 GO에 대한 주소를 담고 있습니다.
* 이를 통해 자신에게 없는 변수는 scope chain을 통해 자신의 상위 VO에서 변수를 검색하며, 최종적으로는 GO에서 변수를 검색하고 없으면 에러를 발생시킵니다.
  * 즉, **변수를 찾는 메커니즘은 scope chain 에 의해 이루어진다고 볼 수 있습니다.**
  * 여기서 주의할 점은 객체의 property를 찾는 것은 아닙니다.
  * 객체의 property 를 찾는 메커니즘은 정확히는 **prototype chain**에 의해서 검색하게 됩니다.
<br />
<br />

### thisValue

<p style="text-align: center;"><img src="execution_context_6.png" /></p>
<div style="display: flex; justify-content: center;">[ thisValue ]</div>

* this 값이 할당됩니다.
* 여기서 this는 이전 포스팅에도 언급했지만, 함수 호출 패턴에 의해 결정되며, 최초 들어있는 값은 GO 의 주소를 담고 있습니다.
<br />
<br />