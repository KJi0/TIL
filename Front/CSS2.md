# Front - CSS2

## 1. display와 flex

### display

웹페이지의 레이아웃을 결정하는 중요한 속성

**값:**

[Block 계열]

- block
- inline
- inline-block

[Box 계열]

- flex
- grid

[기타]

- none
- contents
- list-item

| **block** | **inline-block** | **inline** |
| --- | --- | --- |
| 항상 새 줄에서 시작 | 줄바꿈 없음 | 줄바꿈 없음 |
| div, p, h1~h6 | button, input | span, a, strong |
| 크기 지정 가능 | 크기 지정 가능 | 크기 지정 불가 |

### float과 clear

공간 분할 tag인 div는 block 요소인데 가로 방향 배치가 필요한 경우가 많음

단순히 가로 배치를 위해 inline-block을 사용하지는 않음

**float**

원래 의도는 이미지 주변에 텍스트 배치이며 block 요소의 가로 배치에도 사용됨

최근에는 레이아웃 구성을 위해 flex, grid 등이 사용되기 때문에 코드 유지보수를 위해서만 학습 권장

**값:**

- none: 띄우지 않음
- left: 왼쪽으로 띄움
- right: 오른쪽으로 띄움

**clear**

float을 중지시키는 속성

float된 요소의 주변에 배치되지 않고 다음줄로 내려가게 됨

### flex

요소를 효율적으로 배치, 정렬, 분산할 수 있는 1차원 레이아웃

부모 컨테이너(flex container)에서 자식 아이템(flex item)을 배치하는 방법 정의

부모 컨테이너의 display 속성이 flex

flex 아이템들이 배치된 방향의 축을 main axis, 교차축을 cross axis라고 함

---

## 2. position

**position**

요소를 문서 내에 배치하는 방법을 지정하는 속성

값: static(기본), relative(상대위치), absolute(절대위치), fixed(고정위치), sticky(스크롤 고정)

**top, right, bottom, left**

요소의 상하좌우 위치 지정. 음수는 반대 방향

값: auto(기본), 백분율

### position 속성

**static**

기본 속성으로 일반적인 문서 흐름에 따라 배치

block 요소는 top → bottom, inline 요소는 left → right

다른 위치 속성에 영향을 받지 않음

**relative**

일단 요소를 static하게 배치 후 자신의 원래 위치에서 top, bottom, left, right만큼 이동

다른 요소는 여전히 원래의 위치를 차지하고 있음

**absolute**

top, left 등은 요소의 상위 요소 중 “위치가 설정된, 즉 static이 아닌 조상 요소”를 기준으로 배치

대상이 없다면 최종적으로 body 태그를 기준으로 배치

문서 흐름에서 제거되며 다른 요소의 위치에 영향을 주지 않음

**fixed**

요소를 뷰포트 기준으로 특정 위치에 고정

스크롤을 하더라도 항상 같은 위치에 고정되며 문서 흐름에서 제거됨

---

## 3. 반응형 웹

### 반응형 웹

디바이스의 종류에 따라 페이지의 레이아웃 등이 자동으로 재조정되는 것

웹 요소를 디바이스에 맞게 재배치하고 각 요소의 표시 방법만 변경

### 미디어 쿼리

사이트에 접속하는 장치에 따라 특정한 CSS 스타일을 적용하는 방법으로 **특정 미디어에 대해 특정 CSS 적용**

**기본 형태**

<aside>

@media 미디어 유형 [ [and/or/not] 조건 ] …

</aside>

**조건 활용**

viewport 크기 및 방향에 따른 CSS 처리

미디어 쿼리 종단점(break point: 서로 다른 CSS를 적용할 화면의 크기)에 따라 처리

**mobile first:** 일반적으로 모바일에서 제약 사항이 많기 때문에 ‘모바일’ → ‘데스크톱’ 방향으로 속성 재정의

가장 낮은 단계는 특별히 @media를 지정하지 않음

---

## Appendix: Grid

### grid

웹 페이지의 요소들을 2차원의 그리드 형태로 배치하기 위한 레이아웃

아이템을 배치할 때는 먼저 col의 개수를 결정하고 row를 고려

### grid item 정렬

- **align-content**
    
    기본형: align-content: start;
    
- **justify-content**
    
    기본형: justify-content: start;
    
- **align-items**
    
    기본형: align-items: stretch;
    
- **justify-items**
    
    기본형: justify-items: stretch;