# 홈페이지가 사용자에게 보이는 순서를 설명하세요.

브라우저의 주요 기능은 사용자가 선택한 자원을 서버에 요청하고 브라우저에 표시하는 것이다. 자원은 보통 HTML 문서이다.

브라우저가 서버에서 HTML 응답을 받으면, 화면에 표시되기 전에 많은 단계를 거쳐야 하는데 브라우저가 페이지의 초기 출력을 위해 실행해야하는 이 순서를 Critical Rendering Path(CRP)라고 한다.

CRP는 6단계를 거쳐 홈페이지를 사용자에게 보여준다.

![Untitled](https://github.com/moeyg/Front-end-Knowledge/blob/d179b03c1e9f08078d52f52c7051ffcf86dedaf8/Images/Browser-Rendering/Browser-Rendering-1.png)

1. **HTML 파싱 후, DOM(Document Object Model) 트리 구축**
2. **CSS 파싱 후, CSSOM(CSS Object Model) 트리 구축**
3. **JavaScript 실행**
4. **DOM과 CSSOM을 조합하여 렌더트리(Render Tree) 구축**
5. **뷰포트 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기 계산하여 레이아웃 생성 (Layout / Reflow 단계)**
6. **계산한 위치와 크기를 기반으로 화면에 그리기 (페인팅)**

   <br>

## 1. DOM 트리 구축

![Untitled](https://github.com/moeyg/Front-end-Knowledge/blob/2d162b742815bf905a047624a6f499706e4289c8/Images/Browser-Rendering/Browser-Rendering-2.png)

DOM(Document Object Model) 트리는 완전히 구문 분석된 HTML 페이지의 Object 표현이다.

루트 요소 <html> 로 시작하여 페이지의 각 element / text 에 대한 노드가 만들어진다. 다른 요소 내에 중첩된 요소는 자식 노드로 표시되며 각 노드에는 해당 요소의 전체 특성이 포함된다.

HTML의 장점은 부분적으로 실행될 수 있다는 것이다. 페이지에 내용이 표시하기 위해 전체 문서를 로드할 필요가 없다. 그러나 다른 리소스인 CSS와 JS는 페이지 렌더링을 차단할 수 있다.
   
<br>

## 2. CSSOM 트리 구축

![Untitled](https://github.com/moeyg/Front-end-Knowledge/blob/2d162b742815bf905a047624a6f499706e4289c8/Images/Browser-Rendering/Browser-Rendering-3.png)
 
CSSOM(CSS Object Model)은 DOM과 연관된 스타일의 Object 표현이다.

CSSOM은 DOM과 비슷한 방식으로 표현되지만, 명시적 또는 암시적 선언과 상속 여부에 관계없이 각 노드의 관련 스타일로 표시된다.

CSS는 "렌더링 차단 리소스" 로 간주된다.

즉, 먼저 리소스를 완전히 파싱 하지 않으면, 렌더트리를 구성 할 수 없다. CSS는 HTML과 달리 계단식 상속 특성 때문에 부분적으로 실행될 수 없다. 문서의 뒷 부분에 정의된 스타일은 이전에 정의된 스타일을 무시하고 변경할 수 있다.

따라서 스타일 시트 전체가 파싱 되기 전에 스타일 시트에서 앞에서 정의한 CSS 스타일을 사용하기 시작하면 잘못된 CSS가 적용되는 상황이 발생할 수 있다.

즉, 다음 단계로 넘어 가기 전에 CSS를 완전히 파싱 해야 한다.
<br>

## 3. **JavaScript 실행**

JavaScript는 "파서 차단 리소스"로 간주된다.

즉, HTML 문서 자체의 구문 분석은 JavaScript에 의해 차단된다. 파서가 <script> 태그에 도달하면 (외부 태그 인 경우) fetch를 중단하고 실행한다. 따라서 문서 내의 요소를 참조하는 JavaScript 파일이 있는 경우 해당 문서가 표시된 후에 배치 해야 한다.

JavaScript가 파서 차단되는 것을 피하기 위해 `<script async src="script.js">` 와 같이 `async` 속성을 적용하여 비동기적으로 로드 할 수 있다.
<br>

## 4. 렌더트리 구축

렌더트리는 DOM과 CSSOM의 조합이다. 페이지에서 최종적으로 렌더링 될 내용을 나타내는 트리다.

즉, 표시되는 내용만 캡쳐하기 때문에 `display:none`을 사용하여 CSS로 숨겨진 요소는 포함하지 않는다.

![Untitled](https://github.com/moeyg/Front-end-Knowledge/blob/2d162b742815bf905a047624a6f499706e4289c8/Images/Browser-Rendering/Browser-Rendering-4.png)

<br>

## 5. 레이아웃 생성

레이아웃은 뷰포트의 크기에 관련된 CSS 스타일에 대한 텍스트에 의해 뷰포트의 크기를 결정한다.

뷰포트 크기는 문서 헤드에 제공된 메타 뷰포트 태그에 의해 결정되거나, 태그가 제공되지 않으면 기본 뷰포트 너비인 980px이 적용된다.
   
<br>

## 6. 페인팅

마지막으로 페인팅 단계에서 페이지의 가시적인 내용을 픽셀로 변환하여 화면에 표시 할 수 있다. 페인트 단계에서 처리에 걸리는 시간은 DOM의 크기와 적용되는 스타일에 따라 다르다.
   
<br>

## 정리

브라우저의 주요 기능은 사용자가 선택한 HTML 문서를 서버에 요청하고 브라우저에 표시하는 것입니다.

브라우저가 서버에서 HTML 응답을 받으면, 화면에 표시되기 전에 많은 단계를 거쳐야 하는데 이 순서를 CRP(Critical Rendering Path)라고 합니다.

CRP는 6단계를 거쳐 홈페이지를 사용자에게 보여줍니다.

**우선 HTML 파싱 후, <html> 태그로 시작하여 페이지의 각 요소에 대한 노드를 만들어 DOM(Document Object Model) 트리를 구축합니다.**

**두 번째로 CSS 파싱 후, 각 노드의 관련 스타일로 표시해 CSSOM(CSS Object Model) 트리를 구축합니다.**

**세 번째로 JavaScript 실행합니다.**

**네 번째로 DOM과 CSSOM을 조합하여 최종적으로 렌더링 될 내용이 담긴 렌더트리(Render Tree)를 구축합니다.**

**다섯 번째로 뷰포트 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기 계산하여 레이아웃을 생성합니다.**

**마지막으로 계산한 위치와 크기를 기반으로 화면에 그려 사용자에게 홈페이지를 보여 줍니다.**
