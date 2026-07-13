# Presentation Generator

HTML 기반 셀프 컨테이닝 프레젠테이션 생성 도구. 빌드 도구 없이 단일 HTML 파일로 슬라이드쇼를 만들고, 브라우저에서 바로 열람하거나 PDF/PPT로 출력할 수 있습니다.

## 프로젝트 구조

```
pptslide/
├── agent.md                        # 프레젠테이션 템플릿 가이드 (사양 + 규격)
├── presentation/
│   └── litellm-intro.html          # LiteLLM 소개 프레젠테이션 (예시)
└── README.md
```

## 특징

- **셀프 컨테이닝** — 외부 빌드 도구, 프레임워크, 패키지 매니저 불필요. HTML 파일 하나로 모든 것이 동작
- **A4 가로 레이아웃** — 브라우저 열람과 PDF 출력에 최적화된 16:9 비율
- **키보드 네비게이션** — `← →` / `↑ ↓` 화살표 키로 슬라이드 이동
- **PDF 다운로드** — `Ctrl+P` 또는 PDF 버튼으로 전체 슬라이드 출력
- **PPT 다운로드** — html2canvas + pptxgenjs로 각 슬라이드 스크린샷을 PPTX 파일로 생성
- **다양한 레이아웃** — 2x2 그리드, 3열 카드, 분할 레이아웃, 코드 블록, 테이블 등 템플릿 제공
- **Lucide SVG 아이콘** — 이모지 대신 벡터 아이콘 사용으로 일관된 appearance

## 빠른 시작

### 1. 기존 프레젠테이션 열기

```bash
open presentation/litellm-intro.html
```

### 2. 새 프레젠테이션 만들기

`agent.md`의 템플릿을 참고하여 `presentation/` 디렉토리에 `.html` 파일을 생성합니다.

```html
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>제목</title>
    <script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/pptxgenjs@3.12.0/dist/pptxgen.bundle.js"></script>
    <style>
        /* agent.md 섹션 2~7의 CSS 복사 */
    </style>
</head>
<body>
    <div class="slides-wrapper">
        <!-- 슬라이드 추가 -->
    </div>
    <!-- 네비게이션 + 버튼 (wrapper 밖) -->
    <div class="nav-hint">← → 키로 슬라이드 이동 | Ctrl+P로 인쇄</div>
    <button class="ppt-download-btn" id="pptBtn" onclick="downloadPPT()">PPT 다운로드</button>
    <button class="pdf-download-btn" onclick="downloadPDF()">PDF 다운로드</button>
    <script>
        /* agent.md 섹션 7의 JS 복사 */
    </script>
</body>
</html>
```

## 사용법

### Step 1. 레포지토리 클론

```bash
git clone <repo-url>
cd pptslide
```

### Step 2. AI에게 프롬프트 입력

`agent.md`를 참조하여 주제에 맞는 발표 자료를 요청합니다.

```
agent.md를 참조해서 {{주제}}에 대한 발표용 문서 초안을 작성해줘
```

**예시:**

```
agent.md를 참조해서 Rust 언어의 장점과 도입 사례에 대한 발표용 문서 초안을 작성해줘
```

```
agent.md를 참조해서 마이크로서비스 아키텍처 전환 가이드 발표 자료를 만들어줘
```

```
agent.md를 참조해서 우리 팀의 CI/CD 파이프라인 개선 방안에 대한 발표를 작성해줘
```

### Step 3. 결과 확인 및 출력

AI가 `presentation/` 디렉토리에 HTML 파일을 생성합니다.

```bash
# 브라우저에서 열기
open presentation/*.html
```

| 출력 방법 | 방법 |
|-----------|------|
| 브라우저 열람 | `← →` 화살표 키로 슬라이드 이동 |
| PDF 다운로드 | 우측 상단 **PDF 다운로드** 버튼 또는 `Ctrl+P` |
| PPT 다운로드 | 우측 상단 **PPT 다운로드** 버튼 (스크린샷 기반) |

## 슬라이드 유형

| 유형 | CSS 클래스 | 용도 |
|------|-----------|------|
| 시작 화면 | `.slide-1` | 타이틀, 부제목, 배지 |
| 개요 그리드 | `.slide-2` 등 | 2x2 카드형 소개 |
| 피처 그리드 | `.slide-3` 등 | 3열 기능 목록 |
| 블록 다이어그램 | `.slide-4` | 아키텍처 시각화 |
| 스크린샷 | `.slide-5`~`.slide-8` | 전폭 이미지 |
| 코드 블록 | `.slide-9` | CLI 명령어, 코드 예시 |
| 분할 레이아웃 | `.slide-11` | 2열 비교 |
| 테크 그리드 | `.slide-12`~`.slide-13` | 2x2 기술 상세 |
| LLM 테이블 | `.slide-llm` | 제공사 목록 |
| 종료 화면 | `.slide-15` | 총정리, 핵심 장점 |

## 규격

| 항목 | 값 |
|------|-----|
| 페이지 크기 | A4 landscape (297x210mm) |
| 슬라이드 패딩 | `60px 80px` (상하 60px, 좌우 80px) |
| 타이틀 폰트 | 36px / 700 weight |
| 본문 폰트 | 18px |
| 강조 색상 | `#e94560` (Red) |
| 다크 배경 | `#1a1a2e → #16213e → #0f3460` |

## 참고 자료

- [agent.md](agent.md) — 전체 템플릿 사양, CSS 규격, 레이아웃 패턴, 네비게이션/PDF/PPT 구현 코드
- [html2canvas](https://html2canvas.hertzen.com/) — DOM 캡처 라이브러리
- [PptxGenJS](https://gitbrent.github.io/PptxGenJS/) — 브라우저 PPTX 생성 라이브러리
