# Front - JavaScript1

## 1. JavaScript

동적 타이핑, 객체 지향, 함수형 프로그래밍까지 지원하는 활용도 높은 스크립트 언어

높은 수준의 추상화로 개발자가 세부 사항을 몰라도 프로그래밍 가능

### HTML에서 JS 사용 선언

**HTML 파일 내에 <Script> 태그를 이용한 영역 표시**

일반적으로 DOM 로딩이 완료된 후 사용하기 위해 <body> 태그 아래에 위치

페이지 내에서만 재사용 가능

**외부 파일 링크**

공통되는 스크립트를 별도의 파일 (.js)로 분리하여 여러 html 페이지에서 재사용 가능

3rd party library 사용 시 필수

CDN (Content Delivery Network) 방식: 웹 서비스를 이용해 Script 제공

**inline 방식**

이벤트를 실행할 때 html 태그 내에 JS 코드 삽입

재사용성 없어서 일반적으로 권장 X

---

## 2. 변수와 자료형

### 변수

JS는 동적 타입 언어: 선언 시가 아니라 값 할당할 때 타입 결정

**변수 선언 시 타입 대신 지시어 사용**

var: 가장 많이 사용**했던** 변수 선언 지시어

혼란 유발하므로 가급적 사용하지 말고 유지보수용으로 알기

let: 현대 JS에서 주로 사용되는 지시어

const: 상수 선언에 사용되는 지시어로 새로운 값이 대입되거나 재선언될 수 없음

**변수의 선언과 값의 할당**

기본 camel case로 작성

선언과 함께 리터럴 할당 가능

let과 const는 해당 scope에서 재선언 불가

### 자료형

변수의 타입 - 기본형, 참조형

**기본형**

number: 정수, 실수 구분 X

string

boolean 

undefined: 변수를 선언만 하고 아직 값을 할당하지 않은 상태

null: 객체가 없음

typeof: 저장된 데이터 타입을 문자열 형태로 리턴

**참조형**

기본형 제외 모든 데이터 타입으로 객체

heap에 실제 객체를 구성

instanceof: 참조 타입의 종류 확인

**JavaScript의 객체**

key, value로 구성된 property의 집합

**Js 배열의 특징**

객체 기반으로 index는 객체의 property 역할

요소들이 연속적 메모리 공간을 갖지 않을 수 있음

필요에 따라 크기가 동적으로 조절됨

---

## 3. 연산자와 제어문

### 연산자

**산술연산**

+, -, *, /, %, **

어떤 타입과도 연산이 가능

피연산자들은 상황에 따라 형 변환 후 연산 진행

NaN과의 연산 결과는 항상 NaN

**비교연산**

동등 비교 연산자: ==, !=

일치 연산자: ===, !==

**논리연산자**

&&, ||

비교와 **할당**이 동시에 발생

연산 결과는 단순히 T/F이 아니라 최종 평가 값이 할당됨

### 제어문

조건문: if 등 Java와 동일

다양한 형태의 for문 제공

- for loop와 for-of
- 객체의 속성 순회를 위한 for-in
    
    객체의 속성을 확인할 때 매우 유용
    

---

## 4. Funtion

**기본 형태**

```jsx
function function_name(parameter_name, ...) {
	. . .
	return value;
}
```

**작성 시 주의점**

parameter: var, let 같은 변수 선언 지시어를 사용하지 않고 이름만 표시하며 로컬 변수 취급

return: 타입은 명시하지 않음

**호출 시 주의점**

argument와 parameter의 개수가 맞지 않아도 호출됨

전달되지 않은 parameter는 undefined로 처리됨

**arrow funtion**

함수 리터럴을 간략하게 사용하기 위한 표기법

```jsx
(parameter list) => {
	// do something
}
```

funtion 키워드를 생략하고 ⇒로 대체