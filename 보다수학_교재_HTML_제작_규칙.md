# 보다수학 교재 HTML 제작 규칙

> 기준 파일: `중학 2-1_Ⅲ-1단원_01 부등식과 그 성질.html` / `02 일차부등식의 풀이.html`  
> 마스터 템플릿: `textbook_guide_design.html`

---

## 1. 기술 스택

### 수식 렌더링 — KaTeX (로컬)
```html
<link rel="stylesheet" href="./katex/katex.min.css">
...
<script defer src="./katex/katex.min.js"></script>
<script defer src="./katex/contrib/auto-render.min.js"
  onload="renderMathInElement(document.body, {
    delimiters: [
      {left: '$', right: '$', display: false},
      {left: '$$', right: '$$', display: true}
    ]
  });">
</script>
```
- CDN 사용 금지. 반드시 `./katex/` 로컬 파일 참조
- 인라인 수식: `$...$` / 블록 수식: `$$...$$`
- KaTeX 기본 1.21em을 body 폰트와 동일하게 고정:
  ```css
  .katex { font-size: 1em; }
  .katex .mbin,
  .katex .mrel { margin-left: 0.06em !important; margin-right: 0.06em !important; }
  ```

### 한국어 어절 줄바꿈 방지
```css
body { word-break: keep-all; }   /* 한국어 어절 중간 줄바꿈 방지 — body 전역 선언 */
```
- 어절(띄어쓰기 단위) 중간에서 줄이 바뀌지 않도록 body에 전역 적용
- 긴 영문자·숫자 연속 문자열이 영역을 벗어날 경우 `overflow-wrap: break-word` 병행 사용

### 폰트
| 용도 | 폰트 |
|------|------|
| 본문 전체 (기본) | 윤고딕 (Yoon Gothic) |
| 숫자 · 배지 · 단원번호 | Pretendard Variable (CDN 허용) |

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable.min.css">
```

---

## 2. 색상 체계

| 용도 | 색상 | HEX |
|------|------|-----|
| 개념익히기 (파랑) | 파랑 | `#338cd7` |
| 유형익히기 (핑크) | 핑크 | `#eb749f` |
| 예제 배지 (초록) | 초록 | `#539e84` |
| 풀이 TIP 배지 | 레드 | `#ff7373` |
| 정답 배지 배경 | 회색 | `#797979` |
| 본문 텍스트 | 다크 네이비 | `#232f39` |
| 보조 텍스트 (쏙·익히기 레이블) | 중간 회색 | `#78797a` |
| 유사문제 유형 설명 | 연회색 | `#9a9a9a` |
| 배경 (body) | 회색 | `#adadad` |

---

## 3. 페이지 레이아웃

### 기본 페이지
```css
.page {
  width: 595px; height: 842px;   /* A4 픽셀 기준 */
  background: white;
  display: flex; flex-direction: column;
  padding: 32px 30px 20px 30px;
  gap: 16px;
  overflow: hidden;              /* 넘침 절대 금지 */
}
```
- 모든 페이지는 `overflow: hidden` — 내용이 넘치면 레이아웃 조정
- `<body>`는 `display:flex; flex-direction:column; align-items:center; gap:20px; padding:20px; background:#adadad`
- 특수 페이지는 `style="..."` 인라인으로 padding/gap 조정:
  - 개념 한눈에 보기: `style="padding-top:36px; gap:10px;"`
  - 빠른정답: `style="padding-top:40px; padding-bottom:30px; gap:12px;"`
  - 예제만 있는 세트 페이지: `style="gap:8px;"`

### 인쇄 설정
```css
@media print {
  @page { size: 595px 842px; margin: 0; }
  * { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
  html, body { width: 595px; margin: 0; padding: 0; background: white !important; gap: 0 !important; }
  .page {
    width: 595px !important; height: 842px !important;
    box-shadow: none !important;
    page-break-after: always; break-after: page;
    page-break-inside: avoid; overflow: hidden; margin: 0 !important;
  }
  .page:last-child { page-break-after: auto; break-after: auto; }
}
```

---

## 4. 페이지 구성 패턴

### 단원별 총 페이지 수
- 개념이 2개인 단원(예: 01, 02): **8페이지**
- 개념이 더 많은 단원(예: 01 — 실제는 9페이지): 개념 수에 따라 조정

### 페이지 역할별 구조

| 페이지 번호 | 역할 | 구성 |
|------------|------|------|
| 001 | 개념1 + 예제1 + 연습문제 | `title` + `cbox` + `practice-sec` |
| 002 | 개념2 + 예제2 + 연습문제 | `cbox` + `practice-sec` |
| 003~N-3 | 예제3~ 세트 (개념 없음) | `set-block` × 2 (페이지당 최대 2세트) |
| N-2 | 개념 한눈에 보기 | `kaynun-hd` + `kaynun-box` |
| N-1 | 쏙쏙 유형 익히기 | `umuri-hd` + `umuri-grid` |
| N | 빠른정답 | `qa2-title-row` + `qa2-body` |

**규칙:**
- 페이지 001에만 `<div class="title">` 포함
- 개념 3 이후 예제는 개념 설명(cbox) 없이 예제 세트만 배치
- 예제 세트 페이지: `.set-block`(flex:1) + `.set-divider`(1px 구분선) 구조

---

## 5. 컴포넌트 상세 규칙

### 5-1. 타이틀 (001 페이지 전용)
```html
<div class="title">
  <span class="title__num">02</span>       <!-- 단원번호, 파랑, Pretendard, 60px -->
  <div class="title__right">
    <div class="title__tag">
      <span class="title__tag-blue">개념</span>
      <span class="title__tag-black">익히기</span>
    </div>
    <div class="title__text">단원명</div>   <!-- 24px, bold -->
  </div>
</div>
```

### 5-2. 개념 박스 (cbox)
```html
<div class="cbox">           <!-- 파란 테두리 border: 2px solid rgba(51,140,215,0.2) -->
  <div class="cbox__left">   <!-- flex:1, gap:36px (개념 여러 개일 때 간격) -->
    <!-- 개념 정의 -->
    <!-- 예제 -->
  </div>
  <div class="cbox__right">  <!-- width:112px, 왼쪽 구분선 -->
    <!-- 참고 note -->
    <!-- 풀이 TIP -->
  </div>
</div>
```
- `cbox__left`의 `gap: 36px` — 개념 여러 개 수직 배치 간격
- `cbox__right`는 112px 고정폭, `border-left: 1px solid #F5F5F5`

### 5-3. 개념 정의 (concept-def)
```html
<div class="concept-def">
  <div class="concept-def__hd">
    <div class="concept-chip">
      <span class="concept-chip__gae">개념</span>   <!-- 회색 배경, 파랑 텍스트 -->
      <span class="concept-chip__n">01</span>        <!-- 파랑 원형 배지 -->
    </div>
    <span class="concept-title">개념 제목</span>     <!-- 13px, weight:500 -->
  </div>
  <div class="concept-body">                         <!-- 11.5px, padding-left:26px -->
    <!-- 내용 -->
  </div>
</div>
```

**개념 내 표 (`concept-table`)**:
- `border-collapse: collapse`, 테두리 `0.5px solid #b9b9b9`
- `th`: 배경 `#ebebeb`, font-weight:400
- 첫/마지막 열 `border-left/right: none` (외곽선 제거)
- 하이라이트 셀: `class="hl"` (배경 `#fff5d3`)

### 5-4. 예제 (ex-wrap)
```html
<div class="ex-wrap">                   <!-- padding: 8px 0 0 16px -->
  <div class="ex-badge">
    <span class="ex-badge__pill">예제</span>   <!-- 초록 원형 배지 -->
    <span class="ex-badge__num">01</span>       <!-- 초록 텍스트, Pretendard, 16px -->
  </div>
  <div class="ex-body">                 <!-- 회색 배경 rgba(245,245,245,0.4), 라운드 10px -->
    <div class="ex-content">
      <div class="ex-q">문제 지문</div>
      <!-- 선지 (아래 규칙 참고) -->
    </div>
    <div class="ans-row">
      <span class="ans-badge">정답</span>
      <span class="ans-text">정답 내용</span>
    </div>
  </div>
</div>
```

**예제 선지 정렬 규칙**:
| 클래스 | 구조 | 사용 조건 |
|--------|------|----------|
| `ex-choices-wrap` | `grid, repeat(3, max-content), gap:4px 18px` | 기본 (소선지 3열) |
| `ex-choices-stack` | `flex-column, gap:2px` | 선지가 긴 경우 (세로 나열) |
| `ex-choices-2col` | `grid, 1fr 1fr, gap:4px 0` | 2열 배치 필요 시 |

### 5-5. 오른쪽 사이드 (참고 + 풀이 TIP)

**참고 (side-note)**:
```html
<div class="side-note">
  <div class="side-note__hd">
    <span class="side-bar"></span>        <!-- 파랑 세로 막대 2.5px -->
    <span class="side-label">참고</span>  <!-- 파랑, 8px, bold -->
  </div>
  <div class="side-body">내용</div>       <!-- 8px, #232f39 -->
</div>
```

**풀이 TIP (tip-box)**:
```html
<div class="tip-box">
  <div class="tip-box__hd">
    <span class="tip-pill">풀이 TIP</span>  <!-- 레드 화살표 꼬리 배지 -->
  </div>
  <div class="tip-body">
    <ul>
      <li>항목 (→ 자동 prefix, #232f39)</li>
    </ul>
  </div>
</div>
```
- `tip-body li::before { content: "→ "; color: #232f39; font-weight:700; }`

### 5-6. 쏙쏙 개념 익히기 섹션 (practice-sec)

**헤더**:
```html
<div class="prac-hd">
  <div class="prac-hd__tab">               <!-- 파랑 3방향 테두리, 오른쪽 pill 모양 -->
    <span class="prac-hd__ss">쏙쏙</span>  <!-- 회색, 10px -->
    <span class="prac-hd__gn">개념</span>  <!-- 파랑, 13px, bold -->
    <span class="prac-hd__ih">익히기</span> <!-- 회색, 10px -->
  </div>
  <div class="prac-hd__line"></div>         <!-- 파랑 가로 선, flex:1 -->
</div>
```

**문제 그리드**:
```html
<div class="prac-grid">          <!-- grid: 1fr auto 1fr -->
  <div class="prob">...</div>    <!-- 쌍둥이 문제 -->
  <div class="prac-sep"></div>   <!-- 1px 세로 구분선 #F5F5F5 -->
  <div class="prob">...</div>    <!-- 유사 문제 -->
</div>
```
- 문제가 1개뿐일 때: `class="prac-grid prac-grid--single"` (`grid-template-columns: 1fr`)

**개별 문제 (prob)**:
```html
<div class="prob">
  <div class="prob__hd">
    <span class="prob__num">03</span>        <!-- 파랑, Pretendard, 20px, bold:800 -->
    <span class="prob__badge">쌍둥이</span>  <!-- 파랑 3방향 테두리 pill 배지 -->
  </div>
  <div class="prob__body">                   <!-- 11px, #232f39, gap:10px, margin-left:3px -->
    <div>지문</div>
    <!-- 선지 (아래 규칙 참고) -->
  </div>
</div>
```

**개별 문제 내 강조 박스 (prob__boogi)**:
```html
<div class="prob__boogi">조건 텍스트</div>
<!-- border: 0.5px solid #dadde0, border-radius:3px, padding:8px 7px, 10px -->
```

**문제 선지 정렬 규칙**:
| 클래스 | 구조 | 사용 조건 |
|--------|------|----------|
| `prob__choices-stack` | `flex-column, gap:3px` | 가장 긴 선지 ≥ 9자 또는 1열이 자연스러울 때 |
| `prob__choices-2col` | `grid 1fr 1fr, gap:8px` | 가장 긴 선지 9자~13자 |
| `prob__choices-3col` | `grid 1fr 1fr 1fr, gap:4px 6px` | 가장 긴 선지 **4자 이상 8자 이하** |
| `prob__choices-wrap` | `flex-wrap, justify-content:space-between` | 선지가 모두 **한 줄에 들어오는 경우** — 간격 균등 분산 |

### 5-7. 쏙쏙 유형 익히기 (umuri)

**헤더**: 구조는 `prac-hd`와 동일, 색상만 핑크(`#eb749f`)

**그리드**: `umuri-grid` — `grid: 1fr auto 1fr`, 가운데 `umuri-sep` (1px, `#f0f0f0`)

**문제 블록 높이 규칙 (컬럼당 문제 수에 따라 결정)**:

| 컬럼당 문제 수 | 블록당 높이 | 컬럼 구현 방법 |
|:-----------:|:---------:|--------------|
| 3개 | **240px** | `umuri-col` → `display:grid; grid-template-rows: 240px 240px 240px;` |
| 2개 | **358px** | `umuri-col` → `display:grid; grid-template-rows: 358px 358px;` |

> 근거: 페이지 가용 높이 ≈ 716px ÷ 3 = 238 ≈ **240px** / ÷ 2 = 358px

`umuri-ws` spacer는 grid 레이아웃 적용 시 불필요하므로 **제거**한다.

**문제 구조**:
```html
<!-- umuri-col에 grid 적용 후 umuri-ws 없이 umuri-prob 3개 나열 -->
<div class="umuri-col" style="display:grid;grid-template-rows:240px 240px 240px;align-items:start;">
  <div class="umuri-prob">
    <div class="umuri-prob__hd">
      <span class="umuri-num">01</span>             <!-- 핑크, Pretendard, 20px, bold:800 -->
      <span class="umuri-desc">유형 설명</span>     <!-- 연회색 #9a9a9a, 7px -->
    </div>
    <div class="umuri-q">지문 + 선지</div>   <!-- 11px, #232f39, line-height:18px -->
  </div>
  <div class="umuri-prob">...</div>
  <div class="umuri-prob">...</div>
</div>
```

**유형 익히기 선지 클래스**:
| 클래스 | 구조 | 사용 조건 |
|--------|------|----------|
| `umuri-choices-stack` | `flex-column, gap:3px` | 가장 긴 선지 ≥ 9자 또는 1열이 자연스러울 때 |
| `umuri-choices-2col` | `grid 1fr 1fr, gap:3px 4px` | 가장 긴 선지 9자~13자 |
| `umuri-choices-3col` | `grid 1fr 1fr 1fr, gap:3px 4px` | 가장 긴 선지 **4자 이상 8자 이하** |
| `umuri-choices-wrap` | `flex-wrap, justify-content:space-between` | 선지가 모두 **한 줄에 들어오는 경우** — 간격 균등 분산 |

### 5-8. 개념 한눈에 보기 (kaynun)

```html
<div class="kaynun-hd">                         <!-- 파랑 배경, border-radius:6px -->
  <span class="kaynun-hd__small">한눈에 보는</span>  <!-- white, 10px -->
  <span class="kaynun-hd__big">개념 정리</span>      <!-- white, 13px, bold -->
</div>

<div class="kaynun-box">                        <!-- 파란 테두리, margin-top:-1px, margin-bottom:10px -->
  <div class="kaynun-sec">
    <div class="kaynun-sec__hd">
      <span class="kaynun-chip">개념 01</span>  <!-- 파랑 원형 pill, white, 9px -->
      <span class="kaynun-title">제목</span>    <!-- 14px, weight:500 -->
    </div>
    <div class="kaynun-body">                   <!-- padding-left:33px, 11px -->
      내용 (ul 목록, 표 등)
    </div>
  </div>
</div>
```

**한눈에 보기 표 (`kaynun-table`)**:
- `font-size: 8.5px`, `th` 배경 `#f2f2f2`, `font-weight:500`
- 테두리 `0.5px solid #b9b9b9`, 첫/마지막 열 `border: none`

### 5-9. 빠른정답 (qa2)

```html
<!-- 타이틀 행 -->
<div class="qa2-title-row">
  <span class="qa2-num">02</span>           <!-- 파랑, Pretendard, 26px, bold:700 -->
  <span class="qa2-name">단원명</span>
  <span class="qa2-badge">빠른 정답</span>  <!-- 파랑 배경, white, 9px -->
</div>
<div class="qa2-divider"></div>             <!-- 2px #f5f5f5 구분선 -->

<!-- 본문: 좌측 정답 박스들 + 우측 빈 공간 -->
<div class="qa2-body">
  <div class="qa2-left">    <!-- width:256px 고정 -->
    <div class="qa2-box">   <!-- 개념익히기 박스 -->
      <div class="qa2-box__tab">...</div>           <!-- 파랑 pill 탭 -->
      <div class="qa2-rows">
        <div class="qa2-row">                       <!-- height:30px, border-bottom:none -->
          <div class="qa2-rg"><span class="qa2-rnum">01</span><span class="qa2-rans">④</span></div>
          <div class="qa2-rsep"></div>              <!-- 투명, margin:0 6px -->
          <div class="qa2-rg"><span class="qa2-rnum">02</span><span class="qa2-rans">정답</span></div>
        </div>
      </div>
    </div>
    <div class="qa2-box">   <!-- 유형익히기 박스 -->
      <div class="qa2-box__tab qa2-box__tab--pink">...</div>  <!-- 핑크 pill 탭 -->
      ...
    </div>
  </div>
  <div class="qa2-vdivider"></div>  <!-- 2px #f5f5f5 세로 구분선 -->
  <div class="qa2-right"></div>     <!-- 빈 공간 (flex:1) -->
</div>
```

**빠른정답 행 구조**: 한 `qa2-row`에 문제 2개 쌍으로 배치, 가운데 `qa2-rsep`(투명 구분자)

---

## 6. 예제 세트 페이지 (003~N-3)

개념 없이 예제+연습문제 세트만 2개씩 배치하는 페이지:

```html
<div class="page" style="gap:8px;">

  <div class="set-block">        <!-- flex:1, min-height:0 — 세트 1 -->
    <div class="ex-wrap">...</div>
    <div class="practice-sec">...</div>
  </div>

  <div class="set-divider"></div>  <!-- height:1px, background:#f0f0f0 -->

  <div class="set-block">        <!-- flex:1, min-height:0 — 세트 2 -->
    <div class="ex-wrap">...</div>
    <div class="practice-sec">...</div>
  </div>

  <div class="page-footer">...</div>
</div>
```

- 페이지당 최대 **2세트**
- `set-block`은 `flex:1`로 화면을 균등 분할

---

## 7. 수직선 SVG 규칙

### 문제 본문 내 수직선 (조건 제시용)
- 색상 **사용 가능** — 파랑(`#338cd7`)으로 해 표시
- 크기: `width="160" height="28" viewBox="0 0 160 28"`
- `style="display:block; margin:2px 0 4px;"`

```html
<svg width="160" height="28" viewBox="0 0 160 28" style="display:block;margin:2px 0 4px;">
  <!-- 수직선 기본 라인 -->
  <line x1="8" y1="14" x2="150" y2="14" stroke="#aaa" stroke-width="1"/>
  <polygon points="150,14 144,10 144,18" fill="#aaa"/>
  <!-- 눈금 + 숫자 -->
  <line x1="68" y1="10" x2="68" y2="18" stroke="#aaa" stroke-width="1"/>
  <text x="68" y="26" text-anchor="middle" font-size="8" fill="#555">-2</text>
  <!-- 해 표시 (파랑) -->
  <line x1="68" y1="14" x2="150" y2="14" stroke="#338cd7" stroke-width="2.5"/>
  <polygon points="150,14 144,10 144,18" fill="#338cd7"/>
  <circle cx="68" cy="14" r="4" fill="#338cd7"/>  <!-- ● 등호 포함 -->
</svg>
```

### 선지 수직선 (학생이 고르는 5개 선지)
- 색상 **금지** — 정답이 티나지 않도록 반드시 **모노크롬**
- 크기: `width="90" height="20" viewBox="0 0 90 20"`
- 래퍼: `.numline-wrap` + `.numline-choice`

```html
<div class="numline-wrap" style="gap:3px;">
  <div class="numline-choice">①
    <svg width="90" height="20" viewBox="0 0 90 20">
      <line x1="5" y1="10" x2="85" y2="10" stroke="#aaa" stroke-width="0.8"/>
      <polygon points="85,10 79,7 79,13" fill="#aaa"/>     <!-- 오른쪽 화살표 -->
      <line x1="65" y1="7" x2="65" y2="13" stroke="#aaa" stroke-width="0.8"/>
      <text x="65" y="19" text-anchor="middle" font-size="7" fill="#555">2</text>
      <!-- 해 표시 — 반드시 #555 사용 (파랑 금지) -->
      <line x1="65" y1="10" x2="85" y2="10" stroke="#555" stroke-width="2"/>
      <polygon points="85,10 79,7 79,13" fill="#555"/>
      <circle cx="65" cy="10" r="3" fill="#555"/>          <!-- ● 등호 포함 -->
    </svg>
  </div>
  ...
</div>
```

**등호 포함 여부 표현**:
| 부등호 | 점 | SVG |
|--------|-----|-----|
| ≥, ≤ (포함) | ● 꽉 찬 원 | `fill="#555"` |
| >, < (미포함) | ○ 빈 원 | `fill="none" stroke="#555" stroke-width="1.2"` |

**화살표 방향**:
- 오른쪽 열린: `<polygon points="85,10 79,7 79,13" fill="#aaa"/>` (기본)
- 왼쪽 열린: `<polygon points="5,10 11,7 11,13" fill="#aaa"/>`

---

## 8. 푸터 (page-footer)

```html
<div class="page-footer">
  <span class="page-footer__topic">단원명</span>  <!-- 회색, 8px -->
  <span class="page-footer__num">001</span>        <!-- 다크, 8px, bold -->
</div>
```

**로고 (CSS pseudo-element로 삽입)**:
```css
.page-footer::before {
  content: "";
  width: 59px;
  height: 13px;
  margin-right: auto;          /* 로고를 왼쪽에, 텍스트를 오른쪽에 */
  background: url("./bodamath_logo.png") center / contain no-repeat;
  flex-shrink: 0;
}
```
- 로고 파일: `./bodamath_logo.png` (상대 경로, 반드시 같은 폴더)
- 푸터 전체: `display:flex; gap:4px; align-items:center; justify-content:flex-end; font-size:8px;`

---

## 9. HTML 페이지 주석 규칙

페이지 구분을 주석으로 명확히 표기:
```html
<!-- ═══════════════════════ PAGE 001 ═══════════════════════ -->
<div class="page">
  ...
</div><!-- /page 001 -->
```

세트 내 문제 구분:
```html
<!-- ── Set 1: 예제03 ── -->
<!-- 쌍둥이 05 -->
<!-- 유사 06 -->
```

---

## 10. HTML 특수문자 처리

| 표현 | HTML 코드 |
|------|----------|
| `<` (부등호 미만) | `&lt;` |
| `>` (부등호 초과) | 텍스트 내 직접 사용 가능, 수식 내는 `$>$` |
| 반각 공백 | `&ensp;` |
| 전각 공백 | `&emsp;` |
| 하이픈 마이너스 | `−` (U+2212) or `$-$` |

---

## 11. 파일 구조

```
리브랜딩/
├── 중학 2-1_Ⅲ-1단원_01 부등식과 그 성질.html
├── 중학 2-1_Ⅲ-1단원_02 일차부등식의 풀이.html
├── textbook_guide_design.html       ← 마스터 템플릿 (최신 CSS 기준)
├── bodamath_logo.png                ← 반드시 동일 폴더
└── katex/
    ├── katex.min.css
    ├── katex.min.js
    └── contrib/
        └── auto-render.min.js
```

---

## 12. 콘텐츠 재구성 규칙

교재 HTML의 콘텐츠는 **개념원리 개념 PDF**를 원본으로 삼아 아래 규칙에 따라 재구성한다.

### 개념 설명 (`.cbox` 내 개념 영역)

- **원본**: 개념 PDF의 해당 단원 개념 설명 (개념1, 개념2 … 순서)
- **재구성 범위**: 개념 정의, 핵심 단계(❶❷❸…), 예시 풀이(예), 주의사항 모두 포함
- **적용 방식**:
  - 개념 정의(`.concept-def`) — PDF의 정의 문장을 그대로 사용
  - 핵심 단계가 있는 경우 — ❶❷❸❹ 형식으로 `.concept-body` 내 작성
  - PDF에 **예시 풀이(예)**가 있으면 — 파란 배경 박스로 단계 압축해 삽입:
    ```html
    <div style="background:rgba(51,140,215,0.07); border-radius:4px; padding:5px 8px; display:flex; gap:6px; font-size:10.5px; line-height:17px;">
      <span style="color:#338cd7; font-weight:700; flex-shrink:0;">예</span>
      <div>문제 문장<br>❶ … &ensp;❷ … &ensp;❸ … &ensp;❹ …</div>
    </div>
    ```
  - **주의(주의)** 문구가 있으면 — `.warning-label` 스팬과 함께 개념 바디 마지막에 삽입
- **형광펜(`.hl`)**: PDF에서 강조된 핵심 용어·조건에 적용

### 예제 (`.ex-wrap`)

- **원본**: 개념 PDF의 핵심문제 — 문제 **유형**(풀이 구조)을 기준으로 삼는다
- **재구성 방식**: PDF 핵심문제의 유형을 그대로 유지하되, **문제에 사용된 숫자·소재·예시는 새로 바꿔서** 출제한다 (유형만 차용, 내용은 창작)
- **번호 대응**: PDF 핵심문제 번호와 HTML 예제 번호의 1:1 대응은 **필수가 아님**
  - 특히 단원명에 '활용'이 포함된 단원은 개념과 예제의 대응이 맞지 않을 수 있으므로 아래 배치 규칙을 따른다

#### 예제 배치 규칙 (활용 단원 포함 일반 원칙)

1. **개념 연계 예제 우선 배치**: 개념 설명을 요약했을 때 그 내용과 유형명이 흡사한 예제를 해당 개념 페이지에 연결해 먼저 배치
2. **남은 예제 처리**: 개념 연계 후에도 남은 예제가 있으면, `<개념 한눈에 보기>` 페이지 **직전**에 다음 구조로 삽입:
   - 한 페이지에 **2세트** 배치 (`style="gap:8px;"`)
   - 1세트 = 예제 1개 + 쌍둥이 문제 1개 + 유사 문제 1개
   - 예제는 개념과 연계되지 않으므로 반드시 **유형 레이블** 표시
3. **유형 레이블**: 개념과 직접 연계되지 않는 예제의 문제 지문 맨 끝에 인라인 삽입
   `<span style="font-size:8.5px;color:#539e84;">(유형명)</span>`
4. **그림·도형**: SVG로 재현하거나 `.ex-q` 옆에 인라인 배치

### 연습문제 / 쏙쏙 개념익히기 (`.prac-grid`)

- **원본**: 교재 제작 시 input되는 **문제은행-개념익히기 파일** (단원마다 첨부·제공되는 파일 기준, 고정 파일 아님)
  - 예) `<01 일차부등식의 활용(1)>` → `260428_개념원리 - 중등수학2(상) 108~111p_문제지`
- **사용 방식**: 문제은행 파일의 문제를 **내용 그대로** 사용 (문장·숫자·선지 변경 없음)
- **배치 순서**: 각 문제에 표시된 **유형명**을 참고하여 짝이 되는 예제 아래에 배치
  - 예제 배치 순서가 변경된 경우 문제은행 문제도 그에 맞춰 재배치
  - 쌍둥이·유사 문제 한 쌍을 예제와 같은 set-block 내 `.prac-grid`에 묶음

---

## 13. 새 단원 제작 체크리스트

- [ ] `textbook_guide_design.html` 복사 → 새 파일명으로 저장
- [ ] `<title>` 단원명 변경
- [ ] page 001: 타이틀 단원번호 + 단원명 수정
- [ ] 개념 수에 따라 페이지 001~002 (또는 추가) 구성
- [ ] 개념 3 이후: 세트 페이지 (`set-block × 2`) 구성
- [ ] 개념 한눈에 보기: 개념 수만큼 `kaynun-sec` 추가
- [ ] 쏙쏙 유형 익히기: 6문제 (좌3 + 우3)
- [ ] 빠른정답: 개념익히기 전체 문제 + 유형익히기 6문제
- [ ] 모든 선지 수직선 SVG는 `#555` 모노크롬 (파랑 금지)
- [ ] 문제 본문 수직선은 파랑 `#338cd7` 사용 가능
- [ ] 각 페이지 푸터의 단원명 + 페이지 번호 확인
- [ ] KaTeX 로컬 경로 (`./katex/`) 확인
- [ ] 로고 경로 (`./bodamath_logo.png`) 확인
