---
title: About Web Accessibility
date: 2021-10-11 13:10:30
category: WEB
thumbnail: { thumbnailSrc }
draft: false
---

웹 접근성과 관련해서 학습하면서 느꼈던 점들을 정리해보려고 한다.

먼저, 나는 1년이 조금 넘는 시간동안 프로그래밍을 학습하면서 웹 접근성에 대해서 알지도 못했고 관심도 없었다.

왜 관심이 없었는지 물어본다면, 첫번째로는 HTML을 통해서 마크업을 하는 작업을 중요하게 생각하지 않았고, 둘째는 자바스크립트만 학습하는데도 뇌 용량이 초과했기 때문이라고 답할 수 있겠다.

나는 HTML을 쉽게 생각하고 나중에 필요할때만 찾아서 보면 되겠지라는 안일한 마음으로 HTML을 가볍게 무시했었다.  

하지만, HTML을 본격적으로 학습하면서 이는 엄연한 핑계에 불과했고 프론트엔드 개발자로서 HTML에 대한 지식이 바닥이라는 것은 팥없는 찐빵과 다를바 없다는 것을 깨달았다.

먼저, HTML 웹페이지의 구조를 잡는 도구로 사용되며 웹페에지를 개발하는데 없어서는 안될 존재였다.

HTML은 웹페이지의 구조를 잡는 목적 이외에도 웹 접근성을 높이는 방법으로도 사용될 수 있다.

그렇다면 웹 접근성이란 무엇일까? 개념적으로 정리된 것은 장애를 가지고 있는 사용자들에게 동등한 웹페이지 사용을 제공하기 위한 방법이라고 정의되어 있다.

여기서 장애를 가지고 있다는 것은 신체적으로 가지고 있는 장애 뿐만 아니라 환경적으로나 일시적으로 장애를 갖게 된 경우도 포함된다.

개발을 처음 접한 입장에서 웹 접근성에 대한 부분이 도의적인 측멱이 전부일 수 있지만, 외국에서는 웹 접근성이 좋지 않은 서비스들은 법적으로 처벌을 받을 수 있도록 법제화 되어있다고 한다.

웹 접근성의 목적을 정리해보자면 다음과 같다. 

- 장애인, 고령자 등의 사용자층 확대에 대응
- 규정과 법적 요규 사항에 대한 준수
- 다양한 환경과 새로운 기기에서의 이용 대응
- 개발 및 운용의 효율성 제고
- 사회 공헌 및 복지 기업으로서의 기업 이미지 향상

그럼 지금부터 웹 접근성에 대해서 좀 더 자세하게 알아보겠다.

# Web Accessibility
## WCAG

WAI(Web Accessibility Initiative)라 불리는 단체에서 WCAG라는 웹 접근성에 대한 권고안을 제공하고 있다.

WCAG는 1.0부터 현재 2.2까지 웹 접근성에 대한 가이드라인을 수정 발표하고 있다.

WCAG 2.2는 올해 이내로 발표예정이기 때문에, 현재까지 가장 최신 명세라고 할 수 있는 WCAG 2.1의 가이드 라인을 따르는 것이 올바르다.

- 인식의 용이성
  - 대체 텍스트 제공
  - 미디어에 대한 대안 제공
  - 간단한 형태의 콘텐츠 표현
  - 배경과 전경의 구분
- 운용의 용이성
  - 키보드 제어 가능
  - 콘텐츠의 시간 
  - 발작 유발 콘텐츠 디자인 지양
  - 사용자 탐색 헬퍼 제공
- 이해의 용이성
  - 텍스트 콘텐츠 판독
  - 예측 가능한 웹페이지 구성
  - 사용자의 실수 방지
- 견고성
  - 보조기기 및 다양한 디바이스 호환성

WCAG는 다음 4가지의 지침을 가이드라인으로 삼고 있다.

## 웹 접근성의 실태

웹 접근성 관련 자료들을 수집하면서, 한국지능정보사회진흥원에서 2020년 국내 웹 접근성 실태 조사 보고서를 보게 되었다.

해당 보고서를 통해서 웹페이지에서 잘 지켜지지 않는 가이드라인과 오류 유형들을 확인할 수 있었다.

먼저, 가장 많이 범하는 12가지 오류에 대해서 살펴보겠다.

### 최하위 오류 12가지
#### 1. 적절한 대체 텍스트 제공

- 텍스트가 아닌 콘텐츠의 목적성을 명시해 사용자에게 제공
  - ex) 이미지의 alt 속성
- 문맥을 통해 설명이된 내용에 대해서 대체 텍스트 사용 지양
  - 중복된 내용으로 스크린리더 사용자의 편의성 ↓
- CSS 배경 이미지에 의미가 포함된 경우 IR(Image Replacement) 기법 사용
  - 다른 컨텐츠에 영향을 미치지 않으면서 화면에 보이지 않도록 설정하는 방법

```html
// 장식 목적의 이미지 사용
<img src="valid.jpg" alt="장식"> ❌
<img src="valid.jpg" alt> 👏
```

```html
<a href="#">
	<img src="WeAreTheOne.png" alt> 👏
	We are the one
</a>
```

```css
.ir {
	position: absolute; 👌
	opacity: 0; 👌
	display: none; ❌
	visibility: hidden; ❌
	width:0; ❌
  height: 0; ❌
	font-size: 0; ❌
}
```

#### 2. 반복 영역 건너뛰기

- 반복되는 콘텐츠를 우회할 수 있는 경로 제공

#### 3. 표의 구성

- 화면에 전달하는 정보, 구조 관계는 텍스트로 변환하거나 기계가 인식할 수 있어야 함
- 표는 이해하기 쉡기 구성

```html
<table>
  <caption>
    표의 제목을 설명
  </caption>
  <thead>
    <tr>
      <th scope="col">셀 제목</th>
      <th scope="col">셀 제목</th>
      <th scope="col">셀 제목</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th scope="row">셀 제목</th>
      <td>내용</td>
      <td>내용</td>
    </tr>
    <tr>
      <th scope="row">셀 제목</th>
      <td>내용</td>
      <td>내용</td>
    </tr>
  </tbody>
</table>
```

- 스크린 리더는 `caption`을 해석해 사용자에게 테이블이 무엇을 설명하는 테이블인지 전달
- 스크린 리더는 `th`를 해석해 각 셀이 무엇을 설명하는 셀인지 사용자에게 전달
- 스크린 리더는 `scope` 속성을 통해서 사용자에게 테이블 셀의 관계를 전달
  - `col` → 행
  - `row` → 열

#### 4. 레이블 제공

- 콘텐츠가 사용자 입력을 요구할 때 레이블 또는 설명을 제공
  - ex) `input` 요소의 `placeholder` 속성 대신 `label` 요소 사용

```html
<input value="아이디" /> ❌ <input placeholder="아이디" /> ❌

<input id="user_id" />
<label for="user_id">아이디</label> 👏

<input aria-label="아이디" /> ⭕️
<!-- 권장되지 않는 방법 -->
```

#### 5. 자막 제공

- 녹음된 음성 콘텐츠(멂티미디어 콘텐츠)에 동기화된 자막, 대본 또는 수화를 제공
  - ex) 유튜브 자막
- 대본과 수화는 국내에서 통용
```html
<video poster="myvideo.png" controls>
	<source src="*.mp4" srclang="en" type="video/mp4">
	<track src="*.vtt" kind="captions" srclang="en" label="English"> 
</video>
```

#### 6. 제목 제공

- 웹 페에지는 주제나 목적을 설명하는 제목(title)이 있어야 함
- 제목(heading)과 레이블은 주제 또는 목적을 설명
- 제목(title)은 검색엔진과 스크린리더에게 주요한 영향을 미침
- 제목(heading)은 각 페이지의 제목을 설명할 때 사용
  - 스크린리더 사용자에게 페이지의 목차 제공
- iframe의 제목

```html
<ifram src="*.html"></ifram> ❌

<iframe src="*.html" aria-label="설명"></iframe> 
<iframe src="*.html" aria-label="빈 프레임"></iframe> 
```

#### 7. 정지 기능 제공

- 콘텐츠에 시간 제한이 설정되어 있다면 다음 요소중 하나를 만족
  - 끄기, 조절, 연장
  - ex) 이미지 캐러셀
- 스크린 리더의 사용자는 자동으로 넘어가는 콘텐츠 내용에 취약

#### 8. 키보드 사용 보장

- 콘텐츠의 모든 기능은 개인적인 타이핑 속도에 구애 받지 않고 키보드 인터페이스를 이용하여 조작이 가능해야 함
  - ex) 서브 메뉴, 툴팁
- 마우스에 대한 기본 동작들이 키보드로 접근했을 때 사용 가능하도록
- 장치 독립적 이벤트 핸들러 → 키보드와 마우스의 동등한 사용 보장
  - onblur
  - onchange
  - onclick
  - onfocus
  - oninput
  - onselect

#### 9. 초점 이동

- 키보드 인터페이스를 이용하여 페이지 구성 요소로 포커스 이동이 가능하다면 포커스는 키보드 인터페이스 만으로 구성 요소로부터 떠날 수 있어야 함
- 키보드 조작 가능한 모든 사용자 인터페이스는 키보드 포커스 표시가 보이는 방식으로 제공
  - ex) 텍스트에디터, 로그인화면
  - outline 제거 지양

#### 10. 텍스트 콘텐츠의 명도 대비

- 문자와 문자 이미지의 시각적인 표현은 최소 4.5(전경색):1(배경색)의 명암 대비를 부여

#### 11. 기본 언어 표시

- 모든 웹페이지의 기본 휴먼 랭귀지는 기계적으로 판단할 수 있어야 함
- 웹페이지에서 주로 사용하는 언어를 명시

#### 12. 오류 정정

- 만약 입력 오류가 자동으로 감지되면 오류 항목을 식별하고 사용자에게 문자로 전달하고 의견을 제공
  - ex) 로그인 화면에서 이메일 형식과 맞지 않은 문자 입력 시 오류 제공

## 웹 접근성을 높이는 방법

ARIA 명세에 따라서 마크업을 하면 웹 접근성을 높일 수 있다.

### ARIA

ARIA는 Accessible Rich Internet Application의 약자로 접근 가능한 고기능 인터넷 애플리케이션을 말한다. 

이는 스크린리더와 같이 보조 기기가 알 수 없는 웹페이지에서의 상호작용 효과를 보조기기가 알 수 있도록 접근 가능한 형태로 제공하고 크게 역할, 상태, 속성에 대한 정보를 HTML 태그에 부여해서 보조기기가 접근할 수 있도록 돕는다.

#### ARIA 역할
- `<element role="tablist">` 
- `<element role="tab">` 
- `<element role="tabpanel">`
- `<element role="tooltip">`
- `<element role="status">`
- `<element role="alert">`
- `<element role="alterdialog"> → <dialog>`
- `<element role="dialog"> → <dialog>`
- `<element role="navigation"> → <nav>`
- `<element role="complementary"> → <aside>`
- `<element role="none"> → <div>`

#### ARIA 상태
- `<element aria-current="page|step|location|date|time|true|false(default)">`
- `<element aria-selected="false|true|undefined(default)">`
- `<element aria-haspopup="true|menu|dialog|listbox|tree|grid|false(default)">`
- `<element aria-expanded="true|false|undefined(default)">`
- `<element aria-hidden="true|false|undefined(default)">`
- `<element aria-invalide="true|false(default)|grammar|spelling">`

#### ARIA 속성
- `<element aria-controls="ID reference list">`
- `<element aria-live="polite|assertive|off(default)">`
- `<element aria-labelledby="ID reference list">`
- `<element aria-label="string">`
- `<element aria-describedby="ID reference list">`
- `<element aria-errormessage="ID reference">`
- `<element aria-modal="true|false(default)">`

## 마치며

우선, 웹접근성에 대해서 학습하면서 HTML에 대해서 가볍게 생각하고 넘어갔던 과거의 내 자신에게 반성하는 시간이 되었고 프론트엔드 개발자로 성장하는 과정이 순탄치만은 않다는 것을 느낄 수 있었던 시간이었다.

그리고 웹접근성과 관련해서 정말 많은 참고자료와 이해해야할 개념들이 많아서 아직 정리하지 못한 부분이 많은데 앞으로 웹 접근성과 관련된 학습 내용들을 정리해서 보충하는 시간을 가지려고 한다.

아직 내가 모르는 부분이 많다는 것과 앞으로 학습해야할 기술 지식들에 대해서 불안한 마음이 없지 않지만, 처음 소풍 가는 날을 고대하던 어린이의 마음으로 앞으로 예정된 프론트개발 학습들에 임하려고 한다.

> 참조
>
> [2020 웹접근성 실태 보고서](https://www.nia.or.kr/site/nia_kor/ex/bbs/View.do?cbIdx=99873&bcIdx=23191&parentSeq=23191)
>
> [웹접근성 시리즈](https://dev.to/sargalias/series/11132)
>
> [웹접근성과 웹표준](https://seulbinim.github.io/WSA/)