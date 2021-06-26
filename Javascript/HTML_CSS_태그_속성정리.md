### 텍스트

- `<br>` : 줄바꿈
- `<p>` : 단락을 나눔
- 공백 및 특수 문자 입력
  - `<` : `&lt;`
  - `>` :  `&gt;`
  - `&` : `&amp;`
  - `"` : `&quot;`
  - 공백 :  `&nbsp;`
  - ⓒ : `&copy;`
- `<hr>` : 수평선
- `<h(n)>` : 제목
- `<pre>` : 입력 그대로 표현됨
- `<blockquote>` : 단락을 들여쓰기

### 글자 스타일

- 물리적
  - `<b>` : 진하게
  - `<i>` : 기울이기
  - `<s>` : 취소선
  - `<u>` : 밑줄
  - `<sup>` : 글자가 위첨자로 표시됨
  - `<sub>` : 글자가 아래첨자로 표시됨
  - `<small>` : 작은크기
- 논리적
  - `<samp>` : 샘플을 출력
  - `<var>` : 변수를 표시(기울어짐)
  - `<dnf>` : 용어에 대한 정의(기울어짐)
  - `<em>` : 강조(기울어짐)
  - `<strong>` : 진하게 표시
  - `<abbr>` : 축약형 표시
  - `<address>` : 실제 우편물 주소를 표시(기울어짐, 자동줄바꿈)
  - `<kbd>` : 키보드로 입력한 내용임을 표시
  - `<q>` : 짧은 인용구를 표시(큰따옴표가 붙은형태로 표시)

### 목록

- `<ul>` : 순서없는 목록
  - `<li>`
- `<ol>` : 순서있는목록
  - type : 1, a, A
  - start : 시작 번호
  - reversed : 역행
  - `<li>`
    - value : 중간에 번호를 바꿈
- `<dl>` : 서술목록을 지정
  - `<dt>` : 용어나 이름, `<dd>` : dt에 대한 설명

### 콘텐츠 그룹핑

- 인라인(inline)
  - `<span>`
  - `<img>`
  - `<strong>`
- 블록(block)
  - `<div>`
  - `<p>`
  - `<h1>` 등

### 이미지

- `<img>` : 이미지
- GIF : 256개 색상만 지원, 애니메이션 지원, 용량 작음
- JPG : 트루컬러 지원, 용량이 작음
- PNG : 트루컬러 지원, 투명배경처리 지원, 웹 전용 이미지 파일(jpeg보다 화면상 표시되는 속도가 빠름)

### 하이퍼링크

- `<a>` : 하이퍼링크

### 테이블

- `<table>`

+ `<caption>` : 테이블 이름	

- `<tr>`
- `<thead>` : 그룹핑
  - `<th>`
    - rowspan
    - colspan
- `<tbody>` : 그룹핑
  - `<td>`
    - rowspan
    - colspan
- `<tfoot>` : 그룹핑
- `<colgroup>`, `<col>` : 특정 열의 모든 셀에 대해 스타일 지정

### Audio

- `<audio>` 오디오 재생
- MP3, Wav(익스플로러11 미지원), Ogg(익스플로러11, 사파리9.1 미지원)

### Video

- `<video>` : 비디오 재생
- MP4, WebM(익스플로러11, 사파리9.1 미지원), Ogg(익스플로러11, 사파리9.1 미지원)

### HTML 입력양식

- `<form>`

  - action : 폼데이터 처리할 페이지 주소
  - method : 전송 방식(get : data길이(2048), post : data길이(제한없음)
  - name
  - accept-charset : 문자 인코딩 방식
  - autocomplete : 자동완성 기능 사용여부(on, off)
  - enctype : post방식인 경우 인코딩 방식을 지정
  - novalidate : 데이터의 유혀성을 검사하지 않음
  - target : 처리결과를 보여줄 창을 지정

- `<input>`

  - accept : 파일의 형식 지정(type="file")

  - alt : 이미지 대체 텍스트 (type="image")

  - height, width : (type="image")

  - src : (type="image")

  - autocomplete : 값을 자동으로 완성할지의 여부

  - autofocus : 페이지가 로드될 떄 자동으로 지정한 입력타입으로 포커스를 위치 시킴

  - checked : 선택 여부 (type="checkbox")(type="radio")

  - disabled : 비활성화

  - form="폼_id"

  - formaction, formenctype, formmethod, formnovalidate, formtarget : form 속성 대체

  - list="datalist_id" : input요소에 바인딩되는 datalist 요소(텍스트 박스에 대한 옵션 리스트를 표시)를 지정

  - min, max : 최솟값, 최댓값

  - step : 입력 값의 간격

  - minlenght, maxlenght : 최소 · 최대 문자 개수

  - muliple : 하나 이상의 값을 지정 (type="email")(type="file")

  - name

  - pattern : 입력값에 대한 유효성 검사 (file, text, password, tel, search, url, email, date)

  - placeholder : 힌트 표시 (file, text, password, tel, search, url, email)

  - readonly : 읽기 전용

  - required : 입력 타입에 반드시 값이 입력되어야 함

  - size : 입력 최대 길이

  - type

    - text
    - password
    - radio
    - checkbox
    - hidden
    - file
    - image
    - submit
    - reset
    - button

    ### html5 추가 타입

    - search
    - tel
    - url
    - email : 자동으로 유효성 검사
    - number : 자동으로 유효성 검사
    - range
    - color
    - date
    - month
    - week
    - time
    - datetime
    - datetime-local

  - value

- `<textarea>`

  - name
  - cols
  - rows
  - wrap
  - readonly

- `<select>`

  - name
  - size
  - muliple
  - `<option>`
    - disalbed
    - label
    - selected
    - value

# CSS

- 적용 우선순위 : body태그 내부에 가장 가까이 적용된 스타일 시트가 우선권을 가짐
- 전체 선택자 : *
- 타입 선택자 : 요소명
- 클래스 선택자 : 요소명.클래스명
- 아이디 선택자 : 요소명#아이디명
- 속성 선택자 : 요소명[속성]
- 의사 클래스
  - 링크 의사 클래스
    - :link : 방문하지 않은 링크 적용
    - :visited : 방문한 링크 적용
  - 사용자 동작의사 클래스
    - :hover : 특정 요소를 지정하는 동안 적용 (마우스를 가져다 댔을때)
    - :active : 요소가 활성화 될때 적용
    - :focus : 포커슬르 가지고 있는동안 적용 (마우스로 클릭 했을때)
- 결합자
  - 자손 결합자 : 선택자1 선택자2 → 선택자1 내부에 모든 선택자2
  - 자식 결합자 : 선택자1 > 선택자2 → 선택자1 직계 자손 선택자2
  - 인접 형제 결합자 : 선택자1 + 선택자2 → 선택자1 바로뒤에 선택자2
  - 일반 형제 결합자 : 선택자1 ~ 선택자2 → 선택자1 다음에 있는 모든 선택자2
  - 그룹 결할바 : 선택자1, 선택자2

### CSS 속성

- 색상 : color
- 글꼴 속성
  - font-family : 폰트지정
  - font-size : 폰트사이즈
  - font-style : 글자 모양(normal, italic, oblique)
  - font-weight : 굵기
  - font-variant : 소문자 → 대문자 변경
  - font :
- 텍스트 속성
  - letter-spacing : 문자 사이 간격
  - word-spacing : 단어 사이 간격
  - line-height : 줄간격
  - tab-size : 탭 문자의 크기 지정 (textarea, pre태그 내에서만 적용>
  - text-indent : 첫 번째 줄 들여쓰기(음수는 내어쓰기)
  - text-align : 수평정렬
  - text-align-last : 텍스트의 마지막줄 정렬(justify로만 동작)
  - text-transform : 대문자 또는 소문자로 변경
    - capitalize : 각 단어의 첫글자를 대문자로 변경
    - uppercase : 모두 대문자로 변경
    - lowercase : 모두 소문자로 변경
  - text-decoration : 텍스트의 줄을 그음
    - underline : 밑줄
    - overline : 윗줄
    - line-through :  취소선
  - text-shadow : 텍스트 그림자 지정
    - {text-shadow : <수평> <수직> <흐림(반지름으로 지정)> <그림자색상>}
  - word-wrap : 자동 줄바꿈 (break-word)
- 박스 속성
  - display
    - none : 요소를 화면에 표시X
    - inline : 요소를 인라인박스로 취급
    - block : 요소를 블록 박스로 취급
    - inline-block : 인라인 블록 내부는 블록형식으로 되어 있고 박스 자체는 인라인 박스로 표시
    - list-item : 블록 박스를 목록의 항목과 같이 표시
  - margin : 박스의 외부 여백
  - padding : 요소와 테두리 사이의 여백
    - margin과는 다르게 padding-top, padding-right, padding-bottom, padding-left로 지정 가능
  - width : 폭
  - height : 높이
  - min-width, min-height, max-width, max-height : 최소·최대 높이·폭 을 지정
  - vertical-align : 수직 정렬을 지정할 때 사용
    - baseline : 부모 요소의 기본줄에 맞춰서 정렬 (기본값)
    - sub : 요소를 아래첨자로 정렬
    - super : 요소를 위첨자로 정렬
    - top : 요소의 상단을 해당 줄에서 가장 높은 요소의 상단에 맞춰 정렬
    - middle : 부모 요소의 중앙에 맞춰 정렬
    - bottom : 요소의 하단을 해당 줄에서 가장 낮은 요소의 하단에 맞춰 정렬
    - text-top : 요소의 상단을 부모 요소의 폰트의 상단에 맞춰 정렬
    - text-bottom : 요소의 하단을 부모 요소의 폰트의 하단에 맞춰 정렬
    - <길이>
  - position : top, bottom, left, right와 함께 사용해서 위치 지정
    - static : 위치지정 불가능
    - absolute : 왼쪽상단 모서리를 기준으로 지정한 위치만큼 요소를 배치
    - relative : 현재 요소의 위치를 (0,0)으로 지정하고 이를 기준으로 지정한 위치만큼 이동하여 요소를 배치
    - fixed : 브라우저 윈도우를 기준으로 지정한 위치만큼 이동하여 요소를 배치
  - top, bottom, left, right : position이 static이면 지정해도 변화 없음
  - z-index : 요소끼리 곂쳤을 때 우선순위를 둠
  - float : 요소를 왼쪽이나 오른쪽으로 새롭게 배치함
  - clear : float의 영향을 받지 않고 화면에 표시
  - overflow : 내용이 박스를 넘어갔을 때의 처리방법
    - visible : 오버플로우된 내용 모두 표시
    - hidden : 박스를 벗어난 내용은 표시 X
    - scroll : 오버플로우된 내용을 볼 수 있게 스크롤바 표시
    - auto : 자동으로 스크롤바 표시
  - visibility : 화면에 표시할지에 대한 여부
    - hidden : 화면에 보이지 않지만 공간은 차지
    - collapse : 테이블 요소에서만 행/열 제거, 다른 요소는 hidden 처리
- 배경 속성
  - background-color : 배경색 지정
  - background-image : 배경 이미지 파일 지정(url)
  - background-repeat : 배경 이미지의 반복 여부
  - background-attachment : 배경을 고정할 것인지 스크롤과 같이 움직일 것인지에 대한 여부 지정
    - scroll : 스크롤
    - fixed : 고정
    - local  요소 내부의 콘텐츠 영역과 함께 배경 이미지가 스트롤
  - background-position : 배경 이미지의 시작 위치를 지정
  - background-origin : 배경 이미지가 시작하는 기준 위치를 지정
    - padding-box : 패딩의 왼쪽상단에서 시작
    - border-box : 테두리의 왼쪽 상단에서 시작
    - content-box : 콘텐츠의 왼쪽 상단에서 시작
  - background-size : 배경 이미지의 크기
    - auto : 원래 크기로 표시
    - cover : 이미지의 폭과 높이 중 짧은 것을 기준으로 영역을 채움(크키 비율 무시)
    - contain : 이미지의 폭과 높이 중 긴 것을 기준으로 영역을 채움(크기 비율 유지)
  - background-clip : 배경 속성이 적용되는 영역 지정
    - border-box : 배경속성이 테두리 박스까지 적용
    - padding-box : 패딩 영역까지만 적용
    - content-box : 콘텐츠 영역에서만 적용
- 테두리 속성
  - border-width : 테두리 굵기
    - medium : 중간 굵기
    - thin : 얇은 굵기
    - thick : 두꺼운 굵기
  - border-style : 테두리 스타일
    - none : 테두리 표시 X
    - hidden : 테두리를 숨김
    - dotted : 점선
    - dashed : 대시 형태
    - solid : 실선
    - double : 이중 실선
    - groove : 오목한 선
    - ridge : 볼록한 선
    - inset : 테두리가 안으로 파인 형태
    - outset : 테두리가 밖으로 나온 형태
  - border-color : 테두리 색상
    - transparent : 테두리 색상을 투명하게 지정
  - border-radius : 모서리를 둥굴게 지정
  - box-shadow : 박스의 그림자 스타일 지정
- 목록 속성
  - list-style-type : 항목 마커의 종류를 지정
  - list-style-position : 항목 마커의 위치 지정
  - list-style-image : 항목 마커로서 이미지를 지정
  - list-style : 목록 관련 속성의 일괄 지정
- 테이블 속성
  - table-layout : 셀 안의 내용의 코기에 따른 셀의 크기 변화 여부를 지정
  - border-collapse : 테이블 테두리와 셀의 테두리를 하나의 테두리로 합칠지의 여부를 지정
  - border-spacing : 인접한 셀 테두리 사이의 여백거리를 지정
  - caption-side : 테이블 캡션의 위치를 지정
  - empty-cells : 빈 셀의 테두리와 배경의 표시 여부를 지정