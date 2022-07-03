# 브라우저 렌더링

브라우저의 주요 기능은 사용자가 선택한 자원을 서버에 요청하고 브라우저에 표시하는 것이다. 자원은 보통 HTML 문서이다.

브라우저가 서버에서 HTML 응답을 받으면, 화면에 표시되기 전에 많은 단계를 거쳐야 하는데 브라우저가 페이지의 초기 출력을 위해 실행해야하는 이 순서를 Critical Rendering Path(CRP)라고 한다.

CRP는 6단계를 거쳐 홈페이지를 사용자에게 보여준다.

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/f6f6739b692c54f1356f8a418b7ad26f5444ba0f/Images/Browser-Rendering/Browser-Rendering-1.png" />

1. **우선 렌더링 엔진은 HTML 문서를 파싱하여 <html> 태그로 시작하여 페이지의 각 요소에 대한 노드를 만들어 DOM(Document Object Model) 트리를 구축한다.**

2. **그와 함께 CSS 파싱하여 각 노드의 관련 스타일로 표시해 CSSOM(CSS Object Model) 트리를 구축한다.**

3. **진행 중에 JavaScript를 만나게 되면 작업을 중지하고 JS 엔진에게 제어 권한을 넘기게 되고, JS 엔진은 스크립트를 파싱한다.**

4. **다음으로 DOM과 CSSOM을 조합하여 최종적으로 렌더링 될 내용이 담긴 렌더트리(Render Tree)를 구축한다.**

5. **렌더트리 구축이 끝나면 Reflow 과정에서 뷰포트 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기 계산하여 레이아웃을 생성한다.**

6. **이를 토대로 렌더트리의 노드들을 화면의 올바른 위치에 배치하고 마지막으로 UI Backend가 UI를 페인팅하여 사용자에게 최종적으로 결과 화면을 보여다.**

   <br>

## 1. DOM 트리 구축

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/f6f6739b692c54f1356f8a418b7ad26f5444ba0f/Images/Browser-Rendering/Browser-Rendering-2.png" width="800px" />

DOM(Document Object Model) 트리는 완전히 구문 분석된 HTML 페이지의 Object 표현이다.

루트 요소 <html> 로 시작하여 페이지의 각 element / text 에 대한 노드가 만들어진다. 다른 요소 내에 중첩된 요소는 자식 노드로 표시되며 각 노드에는 해당 요소의 전체 특성이 포함된다.

HTML의 장점은 부분적으로 실행될 수 있다는 것이다. 페이지에 내용이 표시하기 위해 전체 문서를 로드할 필요가 없다. 그러나 다른 리소스인 CSS와 JS는 페이지 렌더링을 차단할 수 있다.
   
<br>

## 2. CSSOM 트리 구축

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/f6f6739b692c54f1356f8a418b7ad26f5444ba0f/Images/Browser-Rendering/Browser-Rendering-3.png" width="800px" />
 
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

<img src="https://github.com/moeyg/Front-end-Knowledge/blob/f6f6739b692c54f1356f8a418b7ad26f5444ba0f/Images/Browser-Rendering/Browser-Rendering-4.png" width="500px" />

<br>
<br>

## 5. 레이아웃 생성

레이아웃은 뷰포트의 크기에 관련된 CSS 스타일에 대한 텍스트에 의해 뷰포트의 크기를 결정한다.

뷰포트 크기는 문서 헤드에 제공된 메타 뷰포트 태그에 의해 결정되거나, 태그가 제공되지 않으면 기본 뷰포트 너비인 980px이 적용된다.
   
<br>

## 6. 페인팅

마지막으로 페인팅 단계에서 페이지의 가시적인 내용을 픽셀로 변환하여 화면에 표시 할 수 있다. 페인트 단계에서 처리에 걸리는 시간은 DOM의 크기와 적용되는 스타일에 따라 다르다.
   
<br>

## 정리

우선 사용자가 브라우저에 URL을 입력하면, 브라우저 엔진은 쿠키나 세션에 원하는 데이터가 캐싱이 되어있는지 확인합니다. 만약 요청한 정보가 있으면 렌더링 엔진에게 바로 보냅니다. 하지만, 요청한 정보가 없다면 도메인네임시스템(DNS)에 찾아가서 IP 주소를 요청하여 받은 IP 주소의 서버를 찾아가서 자료를 요청하고 데이터를 받아와 렌더링 엔진에게 전달합니다.

 브라우저가 서버에서 HTML 응답을 받으면, 화면에 표시되기 전에 일련의 단계를 거쳐야 합니다.

 우선 렌더링 엔진은 HTML 문서를 파싱하여 태그로 시작하여 페이지의 각 요소에 대한 노드를 만들어 **DOM(Document Object Model) 트리**를 구축합니다.

 그와 함께 CSS 파싱하여 각 노드의 관련 스타일로 표시해 **CSSOM(CSS Object Model) 트리**를 구축합니다.

 진행 중에 **JavaScript를 만나게 되면 작업을 중지**하고 **JS 엔진에게 제어 권한을 넘기게 되고**, JS 엔진은 스크립트를 파싱합니다. 더 빠른 화면 출력을 위해 데이터 일부를 받은 후 바로 화면을 표시하고, 그다음에 데이터를 더 받고 다시 화면을 표시하는 것입니다. 

 다음으로 DOM과 CSSOM을 어태치하여 최종적으로 렌더링 될 내용이 담긴 **렌더트리(Render Tree)를 구축**합니다.

 렌더트리 구축이 끝나면 Reflow 과정에서 **뷰포트 기반으로 렌더트리의 각 노드가 가지는 정확한 위치와 크기 계산**하여 레이아웃을 생성합니다. 

 이를 토대로 렌더트리의 노드들을 화면의 **올바른 위치에 배치**하고 마지막으로 UI Backend가 UI를 **페인팅**하여 사용자에게 최종적으로 결과 화면을 보여줍니다.
