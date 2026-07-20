# Presentation Generator

HTML 기반 셀프 컨테이닝 프레젠테이션 생성 도구. 빌드 도구 없이 단일 HTML 파일로 슬라이드쇼를 만들고, 브라우저에서 바로 열람하거나 PDF/PPT로 출력할 수 있습니다.

---

## 프로젝트 구조

```
pptslide/
├── .opencode/
│   └── skills/
│       └── ppt-presentation/
│           └── SKILL.md           # ppt-presentation skill 정의 파일
├── .sisyphus/
│   └── plans/                      # 작업 계획 파일
├── agent.md                        # (v1) 프레젠테이션 템플릿 가이드 (레거시)
├── README.md
└── presentation/
    ├── litellm-intro.html          # (v1) LiteLLM 소개 (레거시 템플릿)
    ├── litellm-intro-v2.html       # (v2) LiteLLM 소개 (Quote-Style)
    ├── github-actions-intro.html   # (v1) GitHub Actions 소개
    ├── ai-senior-advice.html       # (v1) 시니어 개발자 조언
    ├── self-esteem.html            # (v1) 자존감 향상
    └── figma-weave.html            # (v2) Figma Weave 소개 (9슬라이드)
```

---

## 특징

- **셀프 컨테이닝** — 외부 빌드 도구, 프레임워크, 패키지 매니저 불필요. HTML 파일 하나로 모든 것이 동작
- **A4 가로 레이아웃** — 브라우저 열람과 PDF 출력에 최적화된 16:9 비율
- **키보드 네비게이션** — `← →` / `↑ ↓` 화살표 키로 슬라이드 이동
- **PDF 다운로드** — `Ctrl+P` 또는 PDF 버튼으로 전체 슬라이드 출력
- **PPT 다운로드** — html2canvas + pptxgenjs로 각 슬라이드 스크린샷을 PPTX 파일로 생성
- **이미지 ZIP 다운로드** — 모든 슬라이드를 개별 PNG 파일로 ZIP 압축 저장
- **Lucide SVG 아이콘** — 이모지 대신 벡터 아이콘 사용으로 일관된 appearance

---

## 템플릿 버전

### v1 (레거시 — agent.md)
- 기존 템플릿, `agent.md`에 정의
- 다양한 레이아웃 (2x2 그리드, 3열 카드, 분할 레이아웃, 코드 블록 등)
- `litellm-intro.html`, `github-actions-intro.html` 등이 해당

### v2 (Quote-Style — ppt-presentation skill)
- **OpenCode Skill** 시스템 기반 템플릿
- 다크 블루 그라디언트 타이틀/클로징 슬라이드 + 흰색 컨텐츠 슬라이드
- 중앙 정렬 큰 따옴표(quote) 형식의 메시지 전달
- 각 슬라이드는 하나의 핵심 메시지만 전달 (60px quote-text)
- 구조적 순서: `quote-number` → `accent-bar` → `quote-text` → `quote-sub`
- `litellm-intro-v2.html`, `figma-weave.html`이 해당

---

## ppt-presentation Skill 사용법

이 프로젝트는 OpenCode Skill 시스템을 통해 v2 Quote-Style 프레젠테이션을 생성합니다.

### Skill 파일 위치

```
~/.config/opencode/skills/ppt-presentation/SKILL.md
```

또는 프로젝트 내:
```
.opencode/skills/ppt-presentation/SKILL.md
```

### Skill을 통한 생성 (OpenCode AI)

AI에게 프레젠테이션 제작을 요청하면 자동으로 ppt-presentation skill이 로드되어 단일 HTML 파일을 생성합니다.

```
[주제]에 대한 8장 프레젠테이션을 만들어줘
```

예시:
```
Figma Weave에 대한 8장 프레젠테이션을 만들어줘
```

### 수동 생성 (OpenCode 외 환경)

`SKILL.md`의 템플릿 구조를 참고하여 직접 HTML을 작성할 수 있습니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>제목</title>
    <!-- v2 필수 라이브러리 -->
    <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/pptxgenjs@3.12.0/dist/pptxgen.bundle.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/jszip@3.10.1/dist/jszip.min.js"></script>
    <script src="https://unpkg.com/lucide@latest/dist/umd/lucide.js"></script>
    <style>
        /* SKILL.md 섹션 2~7의 CSS */
    </style>
</head>
<body>
    <div class="slides-wrapper">
        <!-- .slide.slide-1 (타이틀) -->
        <!-- .slide.slide-quote (컨텐츠) x N -->
        <!-- .slide.slide-closing (클로징) -->
    </div>
    <!-- 네비게이션 + 다운로드 버튼 -->
    <script>
        /* SKILL.md 섹션 8의 JavaScript */
    </script>
</body>
</html>
```

### v2 슬라이드 구조

| 슬라이드 | CSS 클래스 | 설명 |
|----------|-----------|------|
| 타이틀 | `.slide-1` | 다크 블루 그라디언트 배경, 타이틀 + 부제목 |
| 컨텐츠 | `.slide-quote` | 흰색 배경, 인용구 형식의 핵심 메시지 |
| 종료 | `.slide-closing` | 타이틀과 동일한 배경, 마무리 메시지 |

### CSS 클래스 규칙 (v2)

- 모든 컨텐츠 슬라이드는 `.slide-quote` — 다른 클래스 사용 금지
- 제목 태그(`<h2>` 등) 사용 금지 — `.quote-number` / `.quote-text`로 대체
- 이모지 사용 금지 — `<i data-lucide="...">`만 사용
- 구조 순서: `quote-number` → `accent-bar` → `quote-text` → `quote-sub`

---

## 생성된 프레젠테이션

### Figma Weave 소개 (`figma-weave.html`)

| 항목 | 내용 |
|------|------|
| 템플릿 | v2 Quote-Style |
| 슬라이드 수 | 9장 (타이틀 + 7개 컨텐츠 + 클로징) |
| 주요 내용 | Figma의 Weavy 인수, 노드 기반 워크플로우, 멀티 모델 믹싱, 레이어 편집, Figma 생태계 통합, AI 디자인 플랫폼 통합 |
| 이미지 | weave.figma.com 공식 CDN의 제품 스크린샷 사용 (노드 기반 워크플로우 인터페이스) |
| 생성 과정 | 1. 8장으로 초안 생성 → 2. 전체 구성도 슬라이드 추가 (9장) → 3. 공식 사이트 이미지로 교체 |

### LiteLLM 소개 v2 (`litellm-intro-v2.html`)

| 항목 | 내용 |
|------|------|
| 템플릿 | v2 Quote-Style |
| 슬라이드 수 | 12장 |

---

## 빠른 시작

```bash
# 브라우저에서 열기
open presentation/figma-weave.html

# 키보드 네비게이션
#  - 좌/우 화살표: 이전/다음 슬라이드
#  - Ctrl+P: PDF 출력

# 모든 프레젠테이션 목록
ls presentation/*.html
```
