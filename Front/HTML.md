# Front - HTML

## 1. HTML5 소개

### HTML?

**웹 프로그램**

네트워크 너머의 서버에 존재하며 HTTP를 통해서 서비스되는 프로그램

![image.png](image.png)

**Markup Language**

텍스트에 구조와 의미를 부여하기 위해 **특정 기호, 태그를 사용**하는 언어

프로그래밍보다는 표시용 언어

주로 문서의 형식과 내용을 정의하는 데 사용

- XML, HTML

**HTML**

Hyper Text Markup Language

웹 페이지에서 주로 사용되는 마크업 언어

텍스트뿐 아니라 이미지, 링크 등 다양한 요소를 문서 내에 포함

인터프리터 언어로 웹 프라우저에 의해 해석됨

**HTML5**

단순한 markup → 웹 애플리케이션 플랫폼

결국은 JS가 필요

### HTML 문서의 구조

**<!— 주석 내용 —>:** 주석문

**<!DOCTYPE html>:** 문서의 유형을 나타내는 태그

**<html lang=”ko”>:** 문서의 시작을 여는 태그. lang 속성으로 문서의 언어 지정

**<head>:** 화면에는 보이지 않으나 브라우저가 알아야 할 정보 표시. css, js 등

**<meta charset=”UTF-8”>:** 문자 인코딩 및 문서 키워드

**<title>:** 제목 표시줄에 표시되는 내용으로 방문자나 검색 엔진에 정보 제공

**<body>:** 실제 내용이 들어가는 태그

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>hello html</title>
    <!--TODO: 02. div 요소의 전경색을 red로 설정해보자.-->
     <style>
      div {
        color: red;
      }
     </style>
    <!--END-->
  </head>
  <body>
    <!--TODO: 01. button과 버튼의 클릭 회수를 표현하기 위한 div 태그를 작성해보자.-->
    <button>click me</button>
    <div>click cnt: <span>0</span></div>
    <!--END-->
  </body>
  <!--TODO: 03. 버튼을 클릭할 때마다 클릭된 회수를 증가시키는 이벤트를 작성해보자.-->
   <script>
   let clickCnt = 0;
   document.querySelector("button").addEventListener("click", ()=>{
      console.log("click")
      document.querySelector("span").innerHTML = ++clickCnt
    })
   </script>
  <!--END-->
</html>

```

---

## 2. HTML 기본

### Tag

HTML 문서를 구성하는 요소

- 여는 태그에서 닫는 태그까지의 모든 것을 **element**라고 함
- 빈 요소는 축약형 사용: <br/>, <hr/>
- 일반적으로 <html>이 최상위 root 태그

태그들은 중첩 사용할 수 있으나 꼬이면 안 됨

html 태그는 대소문자 가리지 않음

**block  요소와 inline 요소**

- **Block 요소**
    
    태그를 사용하면 자동적으로 **줄바꿈**이 되는 요소
    
    div, br, ol, ul
    
- **Inline 요소**
    
    한 줄 안에서 다른 인라인 요소와 나란히 표시됨
    
    a, button, img, label, span, textarea
    

### Attribute

**Tag의 속성**

태그에 대한 추가적 정보 제공

여는 태그에 기록하며 name=”value” 형태

일부 속성은 값 없이 선언만으로도 사용 가능 (readonly, selected…)

value는 “” 또는 ‘’를 사용할 수 있으며 보통 “” 사용

여러 개의 속성 사용 가능

```html
<a href=”http://www.naver.com”>네이버로</a>

<input type=”text” value=”hong” readonly />
```

**global 속성**

모든 태그에서 사용할 수 있는 속성

id, class, style, data-*

### 특수문자와 공백

태그를 표현하는 문자(<, >) 등 일부 문자는 예약어로 지정되어 사용 불가

white space는 개수와 상관없이 공백 하나로 인정

사용하기 위해서는 미리 지정된 entity 사용

공백 여러 개 필요하다? → &nbsp;

< → &lt;

> → &gt;

& → &amp;

```html
1
2          3 &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;4

a<b>c, a&lt;b&gt;c

<!--
1 2 3      4
a**c, a<b>c
-->**
```

### emmet

```html
<!--
약어            설명                        사용 예
!               HTML5 문서의 기본 구조 생성
>               하위 요소 생성              header>ul>li
+               동급 요소 생성              section>article>h2+p
*               반복 개수 지정              ul>li*5
# 또는 .        각각 id와 class 지정        ul#menu>li.item*3
( )             그룹핑                     div.container>(header>nav>ul>li*5>a)+(#content>section)+footer
[attribute]     속성 지정                  td[title=name colspan=5]
{ }             텍스트 입력                 a.button{Click Me}
$               넘버링                     ul.list>li.item$*5>{$}
-->
```

---

## 3. 기본 태그

### **Heading**

<h1>~<h6>

제목을 표시하는 태그로 block 요소

검색 엔진은 heading을 이용해 웹 페이지의 구조와 콘텐츠를 색인화하므로 중요

중요도에 따라 h1~h6 사용

### **List**

관련된 아이템들을 목록화

<ul>: Unordered List → 순서가 없는 목록

<ol>: Ordered List → 순서가 있는 목록

<li>: 하위 요소, 목록을 정의함

![image.png](image%201.png)

### **Table**

행과 열을 이용해서 데이터를 구조적으로 저장

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Table</title>
        <style>
        table{
            width: 100%;
            border-collapse: collapse;
        }
        td, th{
            border: 1px solid black;
            padding: 5px;
        }

        table > caption { /* 테이블의 직계 자식인 caption에 color 적용*/
            color: red;
        }
        table > tr { /* table의 직계 자식인 tr에 color 적용*/
            color: red;
        }
        </style>
</head>
<body>
      <!--TODO: 01. thead, tbody, tfoot를 지우고 스타일이 적용되지 않는 이유를 생각해보자.-->
    <table>
      <caption>
        사용자 정보
      </caption>
      <colgroup>
        <col style="background-color: #f0f0f0" />
        <col span="2" style="background-color: #d0e0f0" />
      </colgroup>
        <tr>
          <th>이름</th>
          <th>나이</th>
          <th>키</th>
        </tr>
        <tr>
          <td>홍길동</td>
          <td>25</td>
          <td>175cm</td>
        </tr>
        <tr>
          <td>김철수</td>
          <td>30</td>
          <td>180cm</td>
        </tr>
        <tr>
          <td>평균</td>
          <td>27.5</td>
          <td>177.5cm</td>
        </tr>
    </table>
    <br>
```

**셀 병합**

<td>의 colspan, rowspan 활용

```html
<!--TODO: 02.다음 emmet 표현식을 이용해서 테이블을 생성하고 위의 그림처럼 만들어보자.-->
<table>
  <tbody>
    <tr>
      <th>단</th>
      <th colspan="2">내용</th>
      <tr>
        <td rowspan="9">3단</td>
        <td>3*1</td>
        <td></td>
      </tr>
      <tr>
        <td>3*2</td>
        <td></td>
      </tr>
      <tr>
        <td>3*3</td>
        <td></td>
      </tr>
      <tr>
        <td>3*4</td>
        <td></td>
      </tr>
      <tr>
        <td>3*5</td>
        <td></td>
      </tr>
      <tr>
        <td>3*6</td>
        <td></td>
      </tr>
      <tr>
        <td>3*7</td>
        <td></td>
      </tr>
      <tr>
        <td>3*8</td>
        <td></td>
      </tr>
      <tr>
        <td>3*9</td>
        <td></td>
      </tr>
    </tr>
  </tbody>
</table>

<!--END-->
```

![1 - th 2개 / 2 - td 3개 / 3 - td 2개](image%202.png)

1 - th 2개 / 2 - td 3개 / 3 - td 2개

### image

이미지 표현하기 위해 사용되는 태그

- **src:** 이미지 파일의 경로를 지정
    
    절대경로: /로 시작, 프로젝트 root 기준
    
    상대경로: ./, ../로 지정, 현재 파일 기준
    
    외부 사이트: http://www.~
    
- **alt:** 이미지 설명 텍스트, 이미지가 로드되지 않을 때 대체 텍스트로 사용됨
- **loading:** 이미지 로딩 방식 설정 (예: lazy, eager)

```html
<h1>The img loading attribute</h1>
    <!-- TODO: 01. resource/wedding.jpg와 rocks.jpg를 절대/상대 경로로 출력해보자. -->
    <img src="/resource/wedding.jpg" alt="wedding">
    <img src="../../resource/rocks.jpg" alt="">
    <!--END-->
    <span>안녕?</span>
    <!-- off-screen images -->
    <img src="https:/placecats.com/400/400" loading="lazy" title="400" />
    <img src="https:/placecats.com/401/401" loading="lazy" title="401" />
    <img src="https:/placecats.com/402/402" loading="lazy" title="402" />
```

### link

anchor로 텍스트, 이미지 등을 클릭해서 다른 문서나 위치, 사이트로 이동하는 기능

- href, target, title, download

새창 또는 탭을 결정할 수는 없음

```html
<h1>basic link</h1>
    <!--TODO: 01. a 태그를 이용하여 다음 링크를 작성해보자.-->
    <!--구글(새창) | 싸피(현재창)-->
    <a href="http://www.google.com" target="_blank">구글로</a>
    <a href="http://www.ssafy.com">싸피</a>
    <!--END-->
```

**페이지 내에서 이동**

동일한 페이지 내에 id 속성으로 대상 생성

href에서 #id 형태로 링크 설정

```html
<!--TODO: 02. '처음으로'와 '끝으로' 링크를 만들어보자-->
    <!-- 처음으로 | 끝으로 -->
    <a href="#end">끝으로</a>
    <!--END-->
    <div id="start">
      Lorem ipsum dolor sit amet consectetur adipisicing elit. Dignissimos, cum cupiditate voluptatum recusandae laborum ullam saepe optio non rerum necessitatibus animi beatae rem sunt eveniet. Cum
      ad optio impedit in eius! Deleniti nobis sunt iure vitae porro exercitationem vero nihil tempora impedit magni, aliquam ipsa explicabo a in earum dolore ab veniam, commodi maxime delectus
      adipisci magnam, sed fugit aliquid? Culpa nostrum corporis minima itaque qui totam inventore aliquam, nobis quae cum esse ducimus pariatur optio consequatur quibusdam iusto aperiam enim sed
      deserunt quam nihil facilis tenetur? Nihil similique suscipit repellendus nostrum totam, tenetur non? Consectetur eligendi tempora reprehenderit exercitationem.
    </div>
    <div>
      Voluptas possimus quidem, similique placeat doloribus provident ipsum. Recusandae natus ipsa necessitatibus magni saepe hic labore ipsum modi quo dignissimos, ex dolor quos maxime ut debitis
      provident perspiciatis facilis optio autem tempore veniam nobis numquam dolorem itaque. Commodi, fugiat rem nulla similique deserunt numquam. Nulla molestias ullam quas aut recusandae. Mollitia
      ex numquam excepturi illo est ea? Quia nesciunt aperiam, veniam doloribus est eaque officia magni in reprehenderit nam earum alias vero non nobis a fuga? Nisi, corrupti sequi omnis molestias
      quasi nihil aspernatur totam eum. Facere expedita consectetur cumque hic nostrum odit sit praesentium ullam numquam voluptas! Neque, deleniti.
    </div>
    <div id="end">
      Veniam vel sapiente odio placeat consequuntur quae delectus in minus accusantium eligendi quisquam, ipsa expedita quas quo quod odit maiores culpa perferendis iste doloribus ut dolores! Quam
      quis, nesciunt voluptatem similique illo numquam. Omnis, corporis corrupti! Inventore voluptatibus delectus consectetur corporis, dolore enim libero totam impedit quod assumenda, tempore ratione
      nemo iste laborum eos. Velit harum quidem iusto optio dolor, impedit officiis ab voluptatibus modi dolore quibusdam! Aliquid omnis exercitationem iusto vero voluptatibus perspiciatis, iste
      voluptatem aspernatur delectus quas unde ad ut nemo libero ipsum mollitia odio qui eum suscipit fuga deserunt iure, tempora totam. Corporis magnam porro sit rerum!
    </div>
    <a href="#start">처음으로</a>
    <br />
```

---

## 4. form 활용

### form

사용자로부터 데이터를 입력받아 서버로 전송하기 위해 사용되는 요소

key=value의 형태로 파라미터 전달

### 속성

- **action:** 요청 처리할 서버의 URL 지정
- **method:** 폼 데이터 전송 방법 지정
    - **get:**
    - **post:**
- **enctype:** 폼 데이터 인코딩 방식 지정
- **target:** 폼 제출 후 결과를 열 위치 지정

### <form>의 하위 태그들

![image.png](image%203.png)

**<fieldset>:** 관련된 입력 요소 그룹화

**<legend>:** fieldset의 제목 정의

**<label>:** 입력 필드에 대한 설명이나 이름

**<input>:** 사용자로부터 데이터를 입력받음 (한 줄)

**<textarea>:** 여러 줄의 텍스트 입력받음

**<button>:** 폼 제출 등을 위한 버튼

### input

값을 입력받기 위한 폼 구성 요소

**공통 속성**

**type:** GUI 컴포넌트로 text, password, radio, checkbox…

**id:** 한 화면에서 유일한 값 → 주로 **해당 페이지**에서 사용 (프론트, 클라이언트)

**name:** input 요소의 이름으로 서버에 전달되는 파라미터의 이름 → 주로 **서버**에서 사용

**value:** input 요소의 기본 값으로 서버로 전달되는 파라미터의 값, name과 &로 연결

**input의 타입**

**tel:** 전화번호 입력 필드, 모바일 기기의 가상 키보드 변화

**url:** url 형식 체크 (공백 없고 영문, 마침표, /로 구성)

**email:** 이메일 형식 체크 (@ 필요)

**number:** 숫자를 조절할 수 있는 화살표 표시 → min, max, value

**date/month/week/time:** 사용자 지역을 기준으로 날짜, 월, 주, 시간 입력

**checkbox와 radio 타입**

**checkbox:** 여러 항목 중 0개 이상 ****선택

**radio:** 여러 항목 중 하나만 선택

- 요소 선택의 편의를 위해 반드시 <label>과 연동해서 처리할 것

![image.png](940ef8c2-97cc-465f-a414-cbbde8a5f098.png)

**Button**

**<button type=”submit”>:** 서버로 전송, 기본 타입

**<button type=”reset”>:** 새로 하기

**<button type=”button”>:** js 연동

**Select**

drop down 목록에서 값 선택할 때 사용

### form 전송 방식의 비교

명시적으로 post 방식으로 선언한 경우에만 post, 나머지는 모두 get

**Get**

주소창을 통해 query string으로 파라미터 전달 (ex. page=1&q=today)

브라우저별로 길이 제한

적은 데이터만 전송 가능

데이터 노출될 수 있어 보안에 취약

데이터에 영향을 주지 않는 경우 → 단순 조회

**Post**

바디에 포함

용량 제한 X

데이터 노출 없어 상대적으로 보안 강함

요청 처리가 서버의 데이터에 영향을 줄 경우 사용 → 자료 수정, 삭제, 추가

```html
<!--TODO: 01. 서버로 username과 comment를 전달하기 위한 form을 만들어보자.-->
    <form action="">
      <fieldset>
        <legend>사용자 정보</legend>
        <label for="id">아이디</label>
        <input type="text" id="id" name="userid"><br>
        <label for="comment">comment</label> 
        <textarea name="usercomment" id="comment" cols="30" rows="10"></textarea>
      </fieldset>
      <button formmethod="get">get</button>
      <button formmethod="post">post</button>
    </form>
    <!--END-->
```

---

## APPENDIX: semantic 태그

### **공간 분할 태그**

레이아웃을 구성하고 콘텐츠를 구분하는 태그

**<div>:** 행을 구분, block 형식, 크기 지정 가능, 4방향 margin

**<span>:** 행 구분 없이 inline 형식, 크기 지정 불가, 좌우 margin

기존의 페이지는 영역 구분을 위해 대부분 <div> 사용

일단 <div>로 묶고 해당 영역 구분을 위해 id 사용

→ 어떤 용도로 사용되었는지 파악하기 어렵고 개발자마다 사용하는 id가 다름

### sementic 태그

태그 자체에 의미를 부여

**<header>:** 페이지나 section, article 등의 제목. 일반적으로 로고, 메뉴, 검색창 등과 함께 표시

**<nav>:** 문서 연결하는 네비게이션 링크

**<section>:** 주제별 콘텐츠 영역, 일반적으로 내부에 여러 개의 article 포함

**<article>:** 실제 콘텐츠의 내용 등록

**<footer>:** 문서 말미에 제작정보, 저작권 등의 정보 표시
