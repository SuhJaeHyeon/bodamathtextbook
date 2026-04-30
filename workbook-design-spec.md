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
| 문제 본문 | Pretendard Variable | 11px | 400 | letter-spacing: -0.5px, line-height: 18px |
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

### 타이틀 (`.title`) — 개념이 있는 첫 번째 페이지에만 배치
- 한 단원에 `.cbox`(개념+예제) 페이지가 여러 장일 경우, **타이틀은 첫 번째 페이지에만** 들어간다.
  - 예: 개념01이 (1)~(4)로 쪼개어져 PAGE 001~004에 걸쳐 있으면, `.title`은 PAGE 001에만.
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
- **개념 칩** (`.concept-chip`) — 두 가지 변형:

  **① 일반 개념 칩** (파란 pill + 번호 원)
  - `.concept-chip__gae` — 회색 반타원(`rgba(207,207,207,0.2)`), height 19px, border-radius 0 1000px 1000px 0; 파란 "개념" 텍스트 9px bold; `margin-right: -6px`
  - `.concept-chip__n` — 파란 원, height 19px, min-width 22px, 12px bold white; `z-index: 1`

  **② 보충 개념 칩** (오렌지 pill, 숫자 없음)
  - `.concept-chip__boogi` — 오렌지(`#ff8945`) pill, height 19px, border-radius 0 1000px 1000px 0; "보충 개념" 텍스트 9px bold white; `padding: 3px 9px 3px 6px`
  - ⚠️ **`concept-chip__gae`와 `concept-chip__n` 대신 `concept-chip__boogi` 하나만** 사용
  - ⚠️ **`.page` style은 반드시 `gap:10px;`** — 일반 페이지 기본값(16px)과 다름
  
  ```html
  <!-- 일반 개념 칩 -->
  <div class="concept-chip">
    <span class="concept-chip__gae">개념</span>
    <span class="concept-chip__n">01</span>
  </div>

  <!-- 보충 개념 칩 -->
  <div class="concept-chip">
    <span class="concept-chip__boogi">보충 개념</span>
  </div>
  ```

  ```css
  .concept-chip__boogi {
    background: #ff8945;
    height: 19px; display: flex; align-items: center; justify-content: center;
    border-radius: 0 1000px 1000px 0;
    padding: 3px 9px 3px 6px;
    font-size: 9px; font-weight: 700; color: white;
  }
  ```

- `.concept-title` — Noto Sans KR, 13px, weight 500, letter-spacing: -0.7px
- `.concept-body` — 11.5px, line-height 18px, flex column gap 10px; padding-left 26px
- `.concept-table` — 8px, border 0.5px `#848484`, 헤더 bg `#ebebeb`
- `.hl` — highlight 배경 `#fff5d3`

#### 개념 설명 중 예시 박스 (`.concept-eg`)

- CSS : `rgba(51,140,215,0.06)` bg, border-radius 6px, padding 6px 10px, `display: flex; flex-direction: column; gap: 3px`
- `.concept-eg__label` — "예" 텍스트, 9px weight 700, `#338cd7`
- `.concept-eg__content` — `display: block; white-space: nowrap` — 내용 전체를 하나의 블록으로 감싸서 KaTeX가 생성하는 개별 `<span>` 토큰이 flex 열 아이템으로 분리되는 것을 방지

> **왜 `<div class="concept-eg__content">` 가 필요한가?**
> KaTeX auto-render는 `$...$`를 각각 별도의 `<span>` 으로 변환한다. `.concept-eg`가 `flex-direction: column`이므로, 내용을 래퍼 없이 직접 삽입하면 각 span이 독립 flex row가 되어 토큰 하나씩 줄바꿈되는 문제가 발생한다. `<div class="concept-eg__content">`로 감싸면 KaTeX spans가 해당 div의 인라인 자식으로 흐르고, `white-space: nowrap`이 span 사이의 자동 줄바꿈을 막는다.

**내부 텍스트 줄 구성 원칙**

| 경우 | 기준 | 처리 |
|------|------|------|
| **한 줄** | 조건과 결과가 짧고 한 호흡에 읽히는 경우 | 줄바꿈 없이 한 줄로 작성 |
| **두 줄** | 조건과 계산 과정이 길어 단어 중간에서 줄바꿈 위험이 있는 경우 | `<br>`로 논리적 경계를 명시적으로 지정 |

두 줄 구분 기준:
- **1행** : 주어진 조건 (예: `기울기 $2$, 점 $(1,\,3)$`)
- **2행** : 계산 과정 및 결과 (예: `$\Rightarrow$ $3=2\times1+b$ $\Rightarrow$ $b=1$ $\Rightarrow$ $y=2x+1$`)

```html
<!-- ✅ 한 줄: 조건과 결과가 짧을 때 -->
<div class="concept-eg" style="margin-top:4px;">
  <span class="concept-eg__label">예</span>
  <div class="concept-eg__content">기울기 $2$, $y$절편 $4$ $\Rightarrow$ $y=2x+4$</div>
</div>

<!-- ✅ 두 줄: 조건(1행)과 계산과정(2행)을 <br>로 구분 -->
<div class="concept-eg" style="margin-top:4px;">
  <span class="concept-eg__label">예</span>
  <div class="concept-eg__content">기울기 $2$, 점 $(1,\,3)$<br>$\Rightarrow$ $3=2\times1+b$ $\Rightarrow$ $b=1$ $\Rightarrow$ $y=2x+1$</div>
</div>
```

> ⛔ 내용을 `.concept-eg`에 직접(래퍼 없이) 넣지 않는다 — KaTeX 토큰 분리 문제 발생.
> ⛔ 자동 줄바꿈은 `<br>`로만 제어한다. `.concept-eg__content` 자체의 `white-space: nowrap`은 KaTeX 렌더링 보조 용도이며, 논리적 줄 구분은 반드시 `<br>`로만 지정한다.

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

#### 유형 레이블 — 개념 미연계 예제 전용

- **적용 대상** : `개념정의(.concept-def)`와 같은 `.cbox` 안에 배치되지 않은 예제, 즉 set-block 페이지의 예제 전체
- **위치** : `.ex-q` 텍스트 **맨 끝**, 문장 부호 바로 뒤 공백 한 칸 뒤에 인라인으로 삽입
- **내용** : PDF에서 예제를 추출할 당시 유형명을 **그대로** 사용 (소괄호 안에 넣기)
- **CSS 클래스 없음** — 반드시 인라인 스타일만 사용

```html
<!-- 형식 -->
<div class="ex-q">
  ...문제 텍스트... <span style="font-size:8.5px;color:#539e84;">({유형명})</span>
</div>

<!-- 예시 -->
<div class="ex-q">
  기본 요금이 포함된 비용 문제이다. ... <span style="font-size:8.5px;color:#539e84;">(기본 요금이 포함된 비용 문제)</span>
</div>
```

> ⚠️ **개념정의와 같은 `.cbox`에 배치된 예제**(예 : 예제01, 예제02)에는 유형 레이블을 붙이지 않습니다.

> ⚠️ `.ex-type-label` CSS 클래스는 **사용 금지** — 별도 클래스 정의 없이 인라인 스타일만 사용합니다.

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

> ✅ **풀이 TIP 작성 기준** — 다음 두 가지 중 하나만 작성합니다.
> 1. **핵심 수학 개념** : 이 문제를 풀기 위해 학생이 제일 먼저 떠올려야 하는 개념 (예: "$f(a)$ : $x$ 자리에 $a$ 대입")
> 2. **함정·오답 포인트** : 학생이 놓치거나 틀리기 쉬운 부분
>
> ⛔ **계산 과정·풀이 단계 삽입 금지** — 수식 전개, 수치 대입 결과, 계산 단계 나열은 '문제 풀이 방식'에 해당하므로 작성하지 않습니다. "문제 풀이 방식 : ..." 형태뿐 아니라 계산 과정을 나열한 `<li>` 항목도 삭제합니다.

#### 정답 (`.ans-row`) — `.cbox__right` 안에 위치
- `margin-left: 8px` (추가 들여쓰기)
- `display: flex; flex-direction: row; align-items: center; gap: 4px`
- `.ans-badge` — `#797979` bg, white, 7px bold, border-radius 2px
- `.ans-text` — 11px, weight 500, letter-spacing: -1.1px

### Set-block 페이지 구조 (`.set-block`) — `02 일차부등식의 풀이` 기준

**`.set-block` CSS (필수)**
```css
.set-block { display: flex; flex-direction: column; gap: 16px; }
```
- `.cbox`와 `.practice-sec` 사이 간격을 **16px**로 고정
- 이 CSS가 없으면 개념박스와 연습문제가 붙어서 표시됨

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
| `kaynun-hd__small` 텍스트 | `한눈에 보는` | `개념`, `쏙쏙` |
| `kaynun-hd__big` 텍스트 | `개념 정리` | `한눈에 보기`, `개념` |
| `kaynun-chip` 텍스트 | `01` / `02` | `개념 01` / `개념 02` / `개념 01 (1)` |
| `.page` style | `padding-top:36px; gap:0;` | 기본값 그대로 |
| `.kaynun-title` font-size | `14px` | 13px |

> ⚠️ **`kaynun-hd` HTML 구조 고정** — 자식 요소는 `kaynun-hd__small` + `kaynun-hd__big` 두 개만 사용하며, 텍스트는 아래 템플릿과 **정확히 일치**해야 합니다.
>
> ```html
> <!-- ✅ 유일하게 허용되는 형태 -->
> <div class="kaynun-hd">
>   <span class="kaynun-hd__small">한눈에 보는</span>
>   <span class="kaynun-hd__big">개념 정리</span>
> </div>
> ```

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
- `.prob__body` — 11px, line-height 18px, letter-spacing: -0.5px

**문제 선지 정렬 규칙**:

| 클래스 | CSS 구조 | 사용 조건 |
|--------|---------|----------|
| `prob__choices-stack` | `flex-column, gap:3px` | 가장 긴 선지 ≥ 9자 또는 1열이 자연스러울 때 |
| `prob__choices-2col` | `grid 1fr 1fr, gap:8px` | 가장 긴 선지 9자~13자, 2열 배치가 적합할 때 |
| `prob__choices-3col` | `grid 1fr 1fr 1fr, gap:4px 6px` | 가장 긴 선지 **4자 이상 8자 이하** |
| `prob__choices-wrap` | `flex-wrap, justify-content:space-between` | 선지가 모두 **한 줄에 들어오는 경우** — 선지 간격 균등 분산 |

- `.prob__boogi` — 조건 박스: border 1px `#dadde0`, border-radius 3px, bg `#fafafa`
  - 보기(선지)를 2열로 나열할 때: `prob__boogi` 안에 `display:grid; grid-template-columns:1fr 1fr; column-gap:24px; row-gap:4px;` 내부 div를 사용한다. 같은 줄 항목 사이 **좌우(column) 간격**을 충분히 확보하는 것이 목적이며, `&ensp;` 등 공백 문자로 간격을 흉내 내는 방식은 사용하지 않는다.

### 페이지 푸터 (`.page-footer`)
- **`margin-top: auto`** — `.page`(flex column) 안에서 항상 최하단에 고정. CSS 클래스에 선언하며 인라인 스타일로 중복 추가하지 않는다.
- flex, justify-content: flex-end, gap 4px, font-size 10px
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

#### `.qa2-title-row` 각 요소 고정값

| 요소 | 값 | 비고 |
|------|----|------|
| `.qa2-num` | 단원 번호 (예: `01`) | 페이지 상단 `.title__num`과 동일한 번호 |
| `.qa2-name` | 단원명 텍스트 (예: `연립일차방정식의 활용(1)`) | **스타일은 `.title__text`와 동일** — font-size: 24px, font-weight: 700, color: `#000`, letter-spacing: -1.2px |
| `.qa2-badge` | **텍스트 고정: `빠른 정답`** | 단원명·페이지 번호 등 다른 텍스트 사용 금지 |

```html
<!-- 올바른 예시 -->
<div class="qa2-title-row">
  <span class="qa2-num">01</span>
  <span class="qa2-name">연립일차방정식의 활용(1)</span>   ← 단원명
  <span class="qa2-badge">빠른 정답</span>               ← 고정 텍스트
</div>

<!-- 잘못된 예시 ❌ -->
<span class="qa2-badge">연립일차방정식의 활용(1)</span>  <!-- 단원명 금지 -->
<span class="qa2-badge">빠른정답</span>                  <!-- 띄어쓰기 없는 표기 금지 -->
```

---

### 유형 익히기 페이지 (`.umuri-*`)

**페이지 레이아웃**
- `.umuri-grid` — `display: grid; grid-template-columns: 1fr auto 1fr; gap: 0 10px`
- `.umuri-col` — `display: flex; flex-direction: column; flex: 1`
- `.umuri-sep` — 1px `#f0f0f0` 세로 구분선
- `.umuri-ws` — 컬럼 내 여백: `flex: 1; min-height: 12px`

**문제 칸 (`.umuri-prob`)**
- 높이 고정: `style="height:240px;"` (3문제 컬럼) 또는 `style="height:358px;"` (2문제 컬럼)
- `.umuri-q` — **11px**, line-height **18px**, letter-spacing: -0.5px
- `.umuri-desc` — 7px, weight 600, `#9a9a9a`, 우측 정렬 (문제 유형 레이블)

> ⛔ **`.umuri-diff` (난이도 표시) 사용 금지** — `umuri-prob__hd` 안에 난이도 점(●) 블록을 삽입하지 않습니다. CSS 클래스는 파일에 남아 있어도 HTML 마크업에는 절대 사용하지 않습니다.

**유형 익히기 선지 정렬 규칙**:

| 클래스 | CSS 구조 | 사용 조건 |
|--------|---------|----------|
| `umuri-choices-stack` | `flex-column, gap:2px` | 가장 긴 선지 ≥ 9자 또는 KaTeX 포함 |
| `umuri-choices-2col` | `grid 1fr 1fr, gap:2px 4px` | 가장 긴 선지 9자~13자, 2열 적합할 때 |
| `umuri-choices-3col` | `grid 1fr 1fr 1fr, gap:4px 6px` | 가장 긴 선지 **4자 이상 8자 이하** |
| `umuri-choices-wrap` | `flex-wrap, justify-content:space-between` | 선지가 모두 **한 줄에 들어오는 경우** — 선지 간격 균등 분산 |

> ⚠️ `prob__choices-*` 와 동일한 기준을 따르되 클래스 이름이 `umuri-choices-*` 로 다릅니다.
> ⚠️ 선지는 각각 별도의 `<span>`으로 분리합니다 (`justify-content: space-between` / grid 작동을 위해).

---

## 6. 수식 렌더링

```html
<!-- KaTeX CSS (로컬) -->
<link rel="stylesheet" href="./katex/katex.min.css">

<!-- KaTeX JS + auto-render (로컬, onload 방식) -->
<script defer src="./katex/katex.min.js"></script>
<script defer src="./katex/contrib/auto-render.min.js"
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
- ⚠️ CSS, JS 모두 로컬(`./katex/`) 파일 사용. 네이버 웨일 등 로컬 파일 환경에서 CDN 방식은 수식이 렌더링되지 않을 수 있음.
- ⚠️ `DOMContentLoaded` 리스너 방식 사용 금지 — `onload` 방식만 사용.

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
| 예제 | 개념원리 개념 PDF — 핵심문제 | 번호 1:1 대응; 개념 미연계 예제는 문제 끝에 유형 레이블 삽입 (PDF 추출 당시 유형명을 소괄호에 넣어 인라인 스타일로 표시) |
| 연습문제 | 단원별 문제지 PDF | 번호 1:1 대응 |

---

## 8-1. 콘텐츠 편집 규칙 (HTML 작성 시 반드시 적용)

### 한국어 어절 줄바꿈 방지

- 모든 본문 텍스트에 `word-break: keep-all` 적용 (body 전역 선언)
- 한국어 어절(띄어쓰기 단위) 중간에서 줄이 바뀌지 않도록 함
- 긴 영문자/숫자 연속 문자열이 영역을 벗어날 경우 `overflow-wrap: break-word` 병행 사용

---

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