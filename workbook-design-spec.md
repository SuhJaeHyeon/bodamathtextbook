# 보다수학 워크북 디자인 스펙

> 중학 2-1 Ⅲ-1단원 · 부등식과 그 성질
> **v4 — 참조 파일 2종 엄격 검증 반영 (2026-04-29)**
>
> 참조 기준 파일 :
> - `중학 2-1_Ⅲ-1단원_01 부등식과 그 성질.html` (1차 기준)
> - `중학 2-1_Ⅲ-1단원_02 일차부등식의 풀이.html` (set-block 구조 기준)

---

## 1. 페이지 사이즈

| 항목 | 값 |
|------|-----|
| 용지 | A4 세로 |
| 화면 크기 | 595px × 842px |
| 인쇄 크기 | 595px × 842px (`@page { size: 595px 842px; margin: 0; }`) |
| 외부 배경 | `#adadad` |
| 페이지 배경 | `white` |
| 페이지 간격 (화면) | 20px gap |

### 페이지 내부 여백 (padding)
| 위 | 오른쪽 | 아래 | 왼쪽 |
|----|--------|------|------|
| 32px | 30px | 20px | 30px |

---

## 2. 컬러 팔레트

| 변수 | 값 | 용도 |
|------|----|------|
| `--primary` | `#338cd7` | 주 브랜드 색 (타이틀 숫자, 문제 번호, 개념 칩 등) |
| `--primary-border` | `rgba(51,140,215,0.2)` | 개념 박스 테두리 |
| `--green` | `#539e84` | 예제 뱃지 배경 및 텍스트 |
| `--red-tip` | `#ff7373` | 풀이 TIP 헤더 배경 |
| `--gray-badge` | `#797979` | 정답 뱃지 배경 |
| `--highlight` | `#fff5d3` | 하이라이트 (`.hl`) |
| `--text-strong` | `#232f39` | 기본 텍스트 |
| `--text-gray` | `#848484` | 푸터 단원명 등 보조 텍스트 |
| `--line` | `#dadde0` | 구분선, 테두리 |

---

## 3. 타이포그래피

| 항목 | 폰트 | 크기 | 굵기 | 기타 |
|------|------|------|------|------|
| 기본 폰트 | Pretendard Variable | — | — | CDN: pretendardvariable.min.css |
| 개념 제목 | Noto Sans KR | 13px | 500 | letter-spacing: -0.7px |
| 타이틀 숫자 | Pretendard Variable | 60px | 700 | letter-spacing: -7.2px |
| 타이틀 텍스트 | Pretendard Variable | 24px | 700 | letter-spacing: -1.2px, color: `#000` |
| 예제 번호 | Pretendard Variable | 16px | 700 | color: `--green`, letter-spacing: -1.44px |
| 문제 번호 | Pretendard Variable | 20px | 800 | letter-spacing: -1.8px, color: `--primary` |
| 개념 본문 | Pretendard Variable | 11.5px | 400 | letter-spacing: -0.6px, line-height: 18px |
| 문제 본문 | Pretendard Variable | 12px | 400 | letter-spacing: -0.5px, line-height: 18px |
| 연습문제 보기 | Pretendard Variable | 11px | 400 | line-height: 17px |
| 풀이 TIP 내용 | Pretendard Variable | 8px | 400 | line-height: 1.4 |
| 개념 표 | Pretendard Variable | 8px | — | letter-spacing: -0.48px |
| 참고 본문 | Pretendard Variable | 8px | — | color: `--text-strong`, line-height: 1.4 |
| 수식 | KaTeX 0.16.11 | — | — | CDN auto-render |

---

## 4. 레이아웃 구조 (Figma 기준)

```
.page  (595 × 842px, flex column, gap: 16px, padding: 32 30 20 30)
├── .title                          (페이지 1 전용)
├── .cbox                           (개념+예제+참고+TIP 통합 박스)
│   ├── .cbox__left  (flex: 1)      왼쪽: 개념정의 + 예제
│   │   ├── .concept-def
│   │   └── .ex-wrap
│   │       ├── .ex-badge
│   │       └── .ex-body            ← ex-content > ex-q 만 포함 (ex-sol ❌)
│   │           └── .ex-content
│   │               └── .ex-q
│   └── .cbox__right (width: 112px) 오른쪽: 참고 + 풀이TIP + 정답
│       ├── .side-note              (선택적)
│       ├── .tip-box                (선택적)
│       └── .ans-row                ← ⚠️ 정답은 반드시 cbox__right 안에
└── .practice-sec                   (쏙쏙 개념 익히기 영역)
    ├── .practice-inner             ← prac-hd + prac-grid 모두 여기 안에
    │   ├── .prac-hd                (헤더 라인)
    │   └── .prac-grid              (2열 그리드)
    │       ├── .prob               (쌍둥이 문제)
    │       ├── .prac-sep           (점선 구분)
    │       └── .prob               (유사 문제)
    └── .page-footer
```

> **핵심**: 개념박스 · 예제 · 참고 · 풀이TIP · 정답이 모두 하나의 `.cbox`(파란 테두리 박스) 안에 포함됩니다.
> **금지**: `ex-body` 안에 `ans-row` 또는 `ex-sol` 클래스를 절대 넣지 않습니다. 정답(`.ans-row`)은 항상 `.cbox__right` 안에 위치합니다.

| 변수 | 값 |
|------|----|
| `.cbox__right` 너비 | 112px |
| `.cbox` 내부 열 간격 | 10px |
| `.prac-grid` 열 간격 | 10px |
| `.practice-inner` gap | 14px |

---

## 5. 컴포넌트 상세

### 타이틀 (`.title`) — 페이지 1 전용
- `.title__num` — 60px, weight 700, `#338cd7`, letter-spacing: -7.2px
- `.title__tag` — "개념익히기" 레이블, 8px, weight 700; 파란 "개념" + 검정 "익히기"
- `.title__text` — 24px, weight 700, `#000`, letter-spacing: -1.2px

### 통합 콘텐츠 박스 (`.cbox`)
- `border: 2px solid rgba(51,140,215,0.2)`, `border-radius: 10px`
- `padding: 16px 16px 16px 0` (왼쪽 padding 없음 — 개념 칩이 경계에 닿음)
- `display: flex; gap: 10px`

### 개념 정의 (`.concept-def`)
- `padding-right: 16px`
- `.concept-def__hd` — flex, gap 6px, margin-bottom 10px
- **개념 칩** (`.concept-chip`):
  - `.concept-chip__gae` — 회색 반타원(`rgba(207,207,207,0.2)`), height 19px, border-radius 0 1000px 1000px 0; 파란 "개념" 텍스트 9px bold; `margin-right: -6px`
  - `.concept-chip__n` — 파란 원, height 19px, min-width 22px, 12px bold white; `z-index: 1`
- `.concept-title` — Noto Sans KR, 13px, weight 500, letter-spacing: -0.7px
- `.concept-body` — 11.5px, line-height 18px, flex column gap 10px; padding-left 26px
- `.concept-table` — 8px, border 0.5px `#848484`, 헤더 bg `#ebebeb`
- `.hl` — highlight 배경 `#fff5d3`

### 예제 (`.ex-wrap`)
- `position: relative; padding: 8px 0 0 16px`
- **예제 뱃지** (`.ex-badge`):
  - `position: absolute; top: 0; left: 16px; z-index: 2`
  - `.ex-badge__pill` — `#539e84` pill, white, 9px weight 600, height 18px, border-radius 1000px
  - `.ex-badge__num` — 흰 배경, `#539e84`, 16px weight 700, letter-spacing: -1.44px
- `.ex-body` — `rgba(245,245,245,0.4)` bg, border-radius 10px; `padding: 22px 10px 12px 31px` (top 22px로 뱃지 여백 확보)
  - **내부 구조** : `.ex-content > .ex-q` 만 포함
  - ⛔ **`ex-sol` (풀이 해설) 클래스 사용 금지** — 예제 풀이 해설은 표시하지 않음
  - ⛔ **`ans-row` 를 `ex-body` 안에 넣지 않음** — 정답은 반드시 `.cbox__right` 안에 배치

### 오른쪽 사이드 컬럼 (`.cbox__right`)
- `border-left: 1px dashed #dadde0`, `padding-left: 8px`
- `display: flex; flex-direction: column; gap: 10px`
- 순서 : `.side-note` (선택) → `.tip-box` (선택) → `.ans-row` (필수)

> ⚠️ **tip-box와 ans-row는 항상 cbox__right의 최하단에 위치**해야 한다. 예제 문제는 cbox__left의 하단(개념 정의 아래)에 있으므로, 정답과 TIP이 예제와 시각적으로 나란히 배치되도록 한다.
>
> - `.side-note`가 있는 경우 : `flex: 1`이 자동으로 공간을 채워 tip-box/ans-row를 최하단으로 밀어준다.
> - `.side-note`가 없는 경우 : tip-box 바로 위에 `<div style="flex:1;"></div>` 스페이서를 추가해 동일한 효과를 낸다.
>
> ```html
> <!-- side-note 없는 개념 페이지 예시 -->
> <div class="cbox__right">
>   <div style="flex:1;"></div>   <!-- ← 스페이서: tip-box를 최하단으로 밀기 -->
>   <div class="tip-box">...</div>
>   <div class="ans-row">...</div>
> </div>
> ```

#### 참고 (`.side-note`)
- `.side-note__hd` — flex, gap 3px, height 12px
  - `.side-bar` — 2.5px × 8px 파란 막대, border-radius 1px 4px 4px 1px
  - `.side-label` — 8px bold `#338cd7`
- `.side-body` — 8px, line-height 1.4, color `#232f39`

#### 풀이 TIP (`.tip-box`)
- `.tip-box__hd` — flex, align-items center
- `.tip-pill` CSS (참조 파일 기준 — clip-path 화살표 방식):

```css
.tip-pill {
  background: #ff7373; color: white;
  font-size: 7px; font-weight: 700;
  height: 14px; padding: 0 10px 0 6px;
  display: flex; align-items: center; justify-content: center;
  clip-path: polygon(0 0, 100% 0, calc(100% - 7px) 50%, 100% 100%, 0 100%);
}
.tip-tri { display: none; }   /* tip-tri는 HTML에 두되 CSS로 숨김 */
.tip-body { font-size: 8px; color: #232f39; line-height: 1.4; letter-spacing: -0.4px; padding-left: 8px; }
.tip-body ul { list-style: none; padding: 0; }
.tip-body li::before { content: "→ "; color: #232f39; font-weight: 700; }
```

> ⚠️ `tip-tri`는 HTML 마크업에는 `<span class="tip-tri"></span>`으로 남겨두되, CSS에서 `display: none`으로 숨깁니다. `tip-body li::before` 화살표 색은 `#338cd7`(파란색)이 아니라 **`#232f39`(검정)**입니다.

#### 정답 (`.ans-row`) — `.cbox__right` 안에 위치
- `margin-left: 8px` (추가 들여쓰기)
- `display: flex; flex-direction: row; align-items: center; gap: 4px`
- `.ans-badge` — `#797979` bg, white, 7px bold, border-radius 2px
- `.ans-text` — 11px, weight 500, letter-spacing: -1.1px

### Set-block 페이지 구조 (`.set-block`) — `02 일차부등식의 풀이` 기준

Set-block 페이지는 유형별 문제세트를 담는 페이지입니다.

```
.page  style="gap:8px;"              ← 일반 페이지와 달리 gap을 8px로 줄임
├── .set-block                       (유형 1세트)
│   ├── .cbox                        ← cbox 구조 동일하게 적용
│   │   ├── .cbox__left
│   │   │   └── .ex-wrap             (예제 문제)
│   │   └── .cbox__right
│   │       ├── .tip-box             (선택)
│   │       └── .ans-row
│   └── .practice-sec
│       └── .practice-inner
│           ├── .prac-hd
│           └── .prac-grid
└── .set-block                       (유형 2세트, 필요 시)
    └── ...같은 구조 반복...
```

> ⚠️ **set-block 간 `set-divider` 사용 금지** — 두 set-block 사이에 `<div class="set-divider">` 를 넣지 않습니다.
> ⚠️ **`practice-sec` 안에 반드시 `practice-inner` 감싸기** — `prac-hd`와 `prac-grid`는 항상 `practice-inner` 안에 위치합니다.

---

### 개념 한눈에 보기 페이지 구조 (`.kaynun-*`)

```html
<div class="page" style="padding-top:36px; gap:0;">

  <div class="kaynun-hd">
    <span class="kaynun-hd__small">한눈에 보는</span>   ← ⚠️ "개념" 아님
    <span class="kaynun-hd__big">개념 정리</span>       ← ⚠️ "한눈에 보기" 아님
  </div>

  <div class="kaynun-sec">
    <div class="kaynun-chip">01</div>                   ← ⚠️ "개념 01" 아님, 숫자만
    <div class="kaynun-title">...</div>
    <div class="kaynun-body">...</div>
  </div>

  <div class="kaynun-sec">
    <div class="kaynun-chip">02</div>
    ...
  </div>

</div>
```

| 항목 | 올바른 값 | 잘못된 값 (❌) |
|------|-----------|---------------|
| `kaynun-hd__small` 텍스트 | `한눈에 보는` | `개념` |
| `kaynun-hd__big` 텍스트 | `개념 정리` | `한눈에 보기` |
| `kaynun-chip` 텍스트 | `01` / `02` | `개념 01` / `개념 02` / `개념 01 (1)` |
| `.page` style | `padding-top:36px; gap:0;` | 기본값 그대로 |
| `.kaynun-title` font-size | `14px` | 13px |

> ⚠️ **`.kaynun-chip` 텍스트는 반드시 숫자(두 자리)만 표기한다.** `개념`, `(1)`, `(2)` 등 어떠한 문자도 포함하지 않는다. 개념이 소주제로 분리되어도 chip에는 상위 개념 번호만 표기하고, 소주제 구분은 `.kaynun-title`에서 `(1)`, `(2)` 등으로 표현한다.
>
> ```html
> <!-- 개념01이 (1)(2)로 나뉘는 경우 -->
> <span class="kaynun-chip">01</span>   <!-- ✅ -->
> <span class="kaynun-title">(1) 미지수가 2개인 일차방정식</span>
>
> <span class="kaynun-chip">01</span>   <!-- ✅ -->
> <span class="kaynun-title">(2) 미지수가 2개인 일차방정식 풀기</span>
>
> <!-- 절대 금지 -->
> <span class="kaynun-chip">개념 01 (1)</span>   <!-- ❌ -->
> <span class="kaynun-chip">개념 01</span>        <!-- ❌ -->
> ```

---

### 쏙쏙 개념 익히기 헤더 (`.prac-hd`)
- `.prac-hd__tab` — border-top/right/bottom 2px `#338cd7`, border-left none; border-radius 0 1000px 1000px 0; padding: 2px 9px 4px 5px
  - `.prac-hd__ss` / `.prac-hd__ih` — 10px weight 600, `#78797a`
  - `.prac-hd__gn` — 13px weight 700, `#338cd7`
- `.prac-hd__line` — flex: 1, height 2px, `#338cd7` bg

### 연습문제 그리드 (`.prac-grid`)
- 2열: `grid-template-columns: 1fr auto 1fr`
- 1열(예제 06): `.prac-grid--single { grid-template-columns: 1fr; }`
- `.prac-sep` — 1px dashed `#dadde0` 세로 구분선

### 연습문제 칸 (`.prob`)
- `.prob__hd` — flex, align-items center, gap 2px, margin-bottom 4px
  - `.prob__num` — 20px, weight 800, `#338cd7`, letter-spacing: -1.8px
  - `.prob__badge` — border-top/right/bottom 0.8px `#338cd7`; border-radius 0 1000px 1000px 0; 6px bold `#338cd7`
    - ⚠️ **텍스트 우선순위**: 입력 문제은행 PDF에서 해당 문제에 표기된 태그(`쌍둥이` / `유사`)를 그대로 추출하여 사용
    - PDF에서 태그를 추출할 수 없는 경우에만 기본값 적용 → 왼쪽 열 `쌍둥이`, 오른쪽 열 `유사`
    - 문제 유형명(왕복형·편도형 등) 절대 사용 금지
- `.prob__body` — 12px, line-height 18px, letter-spacing: -0.5px
- `.prob__boogi` — 조건 박스: border 1px `#dadde0`, border-radius 3px, bg `#fafafa`

### 페이지 푸터 (`.page-footer`)
- `margin-top: auto`, flex, justify-content: flex-end, gap 4px, font-size 10px
- `.page-footer__topic` — `#818181`, weight 400
- `.page-footer__num` — `#232f39`, weight 700 (3자리 zero-padding: "001", "002"...)

#### 🔴 로고 삽입 규칙 — 반드시 base64 inline 방식 사용

**잘못된 방식 (Google Drive URL) — 로컬 파일에서 로고가 보이지 않음:**
```css
/* ❌ 외부 URL 방식 — CORS, 네트워크, 캐시 문제로 로고 미표시 */
.page-footer::before {
  background-image:
    url("https://drive.google.com/thumbnail?id=..."),
    url("https://drive.google.com/uc?export=view&id=...");
}
```

**올바른 방식 (base64 inline):**
```css
/* ✅ base64 inline — 인터넷 없이도 항상 표시 */
.page-footer::before {
  content: "";
  display: block;
  width: 59px;
  height: 13px;
  margin-right: auto;
  background-image: url("data:image/png;base64,iVBORw0KGgo...[full base64]...");
  background-position: center;
  background-size: contain;
  background-repeat: no-repeat;
  flex-shrink: 0;
}
```

**로고가 사라지는 원인 (분석):**
1. **Google Drive URL 참조** — 가장 흔한 원인. 로컬 HTML 파일은 외부 URL을 CORS 제한으로 불러오지 못함
2. **스크립트가 `<style>` 블록을 하드코딩으로 재작성** — Python 구조 변경 스크립트가 `<head>` 전체를 새로 쓸 경우 base64를 누락할 수 있음
3. **`head_section` 추출 실패** — 파일에서 head를 잘못 추출해 기존 CSS를 덮어쓸 때 발생

**스크립트 작성 시 필수 패턴:**
```python
# ✅ 기존 head를 항상 참조 파일에서 추출해 재사용
with open('reference_file.html') as f:
    ref = f.read()
head_section = ref[:ref.find('<body>') + len('<body>')]
# → head_section에 base64 로고 CSS가 그대로 보존됨
```

### 빠른정답 페이지 (`.qa2-*`)

```
.page
├── .qa2-title-row          (단원번호 + 단원명 + "빠른정답" 뱃지)
├── .qa2-divider
├── .qa2-body               (flex row)
│   ├── .qa2-left           ← ⚠️ qa2-box는 반드시 여기에만 넣음
│   │   ├── .qa2-box        (쏙쏙 개념 익히기)
│   │   └── .qa2-box        (쏙쏙 유형 익히기, 있을 경우)
│   ├── .qa2-vdivider
│   └── .qa2-right          ← 항상 비워둠 (내용 없음)
└── .page-footer
```

> ⚠️ **`qa2-box`는 `qa2-left`에만** — 개념 익히기, 유형 익히기 등 모든 정답 박스는 예외 없이 `qa2-left` 안에 순서대로 쌓습니다. `qa2-right`는 레이아웃 균형을 위한 빈 컨테이너로만 사용합니다.

| 요소 | 설명 |
|------|------|
| `.qa2-box__tab` | 개념익히기 탭 (파란색 테두리) |
| `.qa2-box__tab--pink` | 유형익히기 탭 (분홍색 테두리) |
| `.qa2-rg` | 문제번호(`.qa2-rnum`) + 정답(`.qa2-rans`) 한 쌍 |
| `.qa2-row` | `qa2-rg` 2개 + `qa2-rsep` 한 줄 |

---

## 6. 수식 렌더링

```html
<!-- KaTeX CSS (로컬) -->
<link rel="stylesheet" href="./katex/katex.min.css">

<!-- KaTeX JS + auto-render (CDN, onload 방식) -->
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.js"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/contrib/auto-render.min.js"
  onload="renderMathInElement(document.body, {
    delimiters: [
      {left: '$$', right: '$$', display: true},
      {left: '$',  right: '$',  display: false}
    ],
    throwOnError: false
  });"></script>
```

- 인라인 수식: `$수식$`
- 블록 수식: `$$수식$$`
- ⚠️ CSS는 로컬(`./katex/katex.min.css`), JS는 CDN + `onload` 방식 사용. `DOMContentLoaded` 리스너 방식 사용 시 수식이 렌더링되지 않을 수 있음.

---

## 7. 인쇄 설정

```css
@media print {
  @page { size: 595px 842px; margin: 0; }
  * { -webkit-print-color-adjust: exact !important; print-color-adjust: exact !important; }
  html, body { width: 595px; margin: 0; padding: 0; background: white !important; gap: 0 !important; }
  .page { width: 595px !important; height: 842px !important; box-shadow: none !important;
          page-break-after: always; break-after: page; page-break-inside: avoid;
          overflow: hidden; margin: 0 !important; }
  .page:last-child { page-break-after: auto; break-after: auto; }
}
```

- 브라우저: `Ctrl+P` → 여백 없음 → PDF로 저장
- 배경 그래픽 옵션 ON 권장 (색상 요소 보존)

---

## 8. 콘텐츠 재구성 원칙

> 상세 규칙은 `보다수학_교재_HTML_제작_규칙.md` **12. 콘텐츠 재구성 규칙** 참고

| 영역 | 원본 소스 | 핵심 원칙 |
|------|-----------|-----------|
| 개념 설명 | 개념원리 개념 PDF — 해당 단원 개념 설명 | 정의·단계·예시 풀이·주의 모두 포함; PDF 예시 풀이(예)는 파란 배경 박스로 삽입 |
| 예제 | 개념원리 개념 PDF — 핵심문제 | 번호 1:1 대응; 개념 미연계 예제는 문제 끝에 유형 레이블 삽입 |
| 연습문제 | 단원별 문제지 PDF | 번호 1:1 대응 |

---

## 8-1. 콘텐츠 편집 규칙 (HTML 작성 시 반드시 적용)

### 콜론 앞 공백

- 텍스트 콘텐츠에서 `:`이 나오는 경우, **반드시 앞에 공백** 추가
- 형식: `단어 :` (단어 뒤 한 칸 띄고 콜론)
- 예) `수직선 표현 :`, `예) $-2<x<1$일 때 :`
- **적용 제외**: CSS 속성값 (`color: red`), HTML 속성, URL (`http://`), KaTeX 수식 내부

---

### 예제 정답 섹션 (`.ans-text`)

- 정답은 **구하는 값만 간결하게** 표기. 서술형 문장 금지.
- 예) `최대 $3\,\text{km}$` (O) &nbsp;&nbsp; `최대 $3\,\text{km}$ 지점까지 갈 수 있다.` (X)
- 소문제 번호 닫는 괄호 뒤에 **`&ensp;`** (en space) 추가
- 형식: `(1)&ensp;$수식$&ensp;(2)&ensp;$수식$`

---

### 참고 (`.side-note`) 작성 규칙

- **핵심문제 번호 출처 표기 금지** — "핵심문제 01, 02" 등의 텍스트를 side-body 첫 줄에 넣지 않습니다.
- 내용은 개념 유형 설명이나 풀이 힌트만 작성합니다.

---

### 개념익히기 문제 조건 박스 (`.prob__boogi`)

- 박스 안 텍스트가 **한 줄**인 경우 → 가운데 정렬 적용
- 적용 방법: `style="justify-content:center; text-align:center;"`
- **여러 줄 텍스트**인 경우 → 기본값 유지 (가운데 정렬 미적용)

---

### 유형 익히기 페이지 (`.umuri-prob`) 높이 고정

- 문제 높이를 **고정값**으로 설정하여 컬럼 내 균등 배치
- 한 컬