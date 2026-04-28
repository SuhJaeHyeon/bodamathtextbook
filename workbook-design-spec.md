# 보다수학 워크북 디자인 스펙

> 중학 2-1 Ⅲ-1단원 · 부등식과 그 성질
> **v3 — Figma 리브랜딩 완전 반영 (2026-04-24)**

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
│   └── .cbox__right (width: 112px) 오른쪽: 참고 + 풀이TIP
│       ├── .side-note
│       └── .tip-box
└── .practice-sec                   (쏙쏙 개념 익히기 영역)
    ├── .practice-inner
    │   ├── .prac-hd                (헤더 라인)
    │   └── .prac-grid              (2열 그리드)
    │       ├── .prob               (쌍둥이 문제)
    │       ├── .prac-sep           (점선 구분)
    │       └── .prob               (유사 문제)
    └── .page-footer
```

> **핵심**: 개념박스 · 예제 · 참고 · 풀이TIP이 모두 하나의 `.cbox`(파란 테두리 박스) 안에 포함됩니다.

| 변수 | 값 |
|------|----|
| `.cbox__right` 너비 | 112px |
| `.cbox` 내부 열 간격 | 10px |
| `.prac-grid` 열 간격 | 10px |

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
- `.ans-row` — flex row, gap 4px, align-items center
  - `.ans-badge` — `#797979` bg, white, 7px bold, border-radius 2px
  - `.ans-text` — 11px, weight 500, letter-spacing: -1.1px

### 오른쪽 사이드 컬럼 (`.cbox__right`)
- `border-left: 1px dashed #dadde0`, `padding-left: 8px`
- `display: flex; flex-direction: column; gap: 10px`

#### 참고 (`.side-note`)
- `.side-note__hd` — flex, gap 3px, height 12px
  - `.side-bar` — 2.5px × 8px 파란 막대, border-radius 1px 4px 4px 1px
  - `.side-label` — 8px bold `#338cd7`
- `.side-body` — 8px, line-height 1.4, color `#232f39`

#### 풀이 TIP (`.tip-box`)
- `.tip-box__hd` — flex, align-items center
  - `.tip-pill` — `#ff7373` bg, white, 7px bold, height 12px
  - `.tip-tri` — CSS 삼각형 (border-trick): `border-left: 6px solid #ff7373`
- `.tip-body` — 8px, line-height 1.4; `li::before { content: "→ "; color: #338cd7; }`

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
- `.prob__body` — 12px, line-height 18px, letter-spacing: -0.5px
- `.prob__boogi` — 조건 박스: border 1px `#dadde0`, border-radius 3px, bg `#fafafa`

### 페이지 푸터 (`.page-footer`)
- `margin-top: auto`, flex, justify-content: flex-end, gap 4px, font-size 10px
- `.page-footer__topic` — `#818181`, weight 400
- `.page-footer__num` — `#232f39`, weight 700 (3자리 zero-padding: "001", "002"...)

### 빠른정답표
- `.qa-title` — 18px bold `#338cd7`, border-bottom 2px `#338cd7`, letter-spacing: -1px
- `.qa-num` — 14px bold `#338cd7`, text-align center, width 38px
- `.qa-ans` — 12px `#232f39`, width 75px
- `.qa-badge` — `#338cd7` bg, white, 16px bold, border-radius 4px

---

## 6. 수식 렌더링

```html
<!-- KaTeX CSS -->
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.11/dist/katex.min.css">

<!-- KaTeX JS + auto-render -->
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


### 예제 정답 섹션 (`.ans-text`, `.side-body`)

- 소문제 번호 닫는 괄호 뒤에 **`&ensp;`** (en space) 추가
- 형식: `(1)&ensp;$수식$&ensp;(2)&ensp;$수식$`
- 예) `<span class="ans-text">(1)&ensp;$x<6$&ensp;(2)&ensp;$x\geq-1$</span>`

> 이유: 소문제 번호와 수식 사이의 시각적 간격을 일정하게 유지하기 위함. HTML 일반 공백은 렌더링 시 무시되거나 너무 좁게 보임.

---

### 개념익히기 문제 조건 박스 (`.prob__boogi`)

- 박스 안 텍스트가 **한 줄**인 경우 → 가운데 정렬 적용
- 적용 방법: `style="justify-content:center; text-align:center;"`
- 예) `<div class="prob__boogi" style="justify-content:center; text-align:center;">한 병에 500원인 주스...</div>`
- **여러 줄 텍스트**인 경우 → 기본값 유지 (가운데 정렬 미적용)

> 이유: 짧은 조건문이 박스 왼쪽에 붙어 있으면 시각적으로 불균형해 보임.

---

### 유형 익히기 페이지 (`.umuri-prob`) 높이 고정

- 문제 높이를 **고정값**으로 설정하여 컬럼 내 균등 배치
- 한 컬럼에 **3개** 배치 시: `style="gap:3px; height:240px; overflow:hidden;"`
- 한 컬럼에 **2개** 배치 시: `style="gap:3px; height:358px; overflow:hidden;"`
- `.umuri-ws` (spacer)가 `flex:1`이므로 고정 높이 문제 사이 여백을 자동 분배

> 이유: 고정 높이 없이 content-fit이면 문제마다 높이가 달라 컬럼이 들쭉날쭉해 보임.

---

## 9. 미구현 (추후 작업)

- `개념마무리` 페이지 타입 (Figma node 13757:149)
