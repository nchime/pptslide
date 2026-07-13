# Presentation Generator Harness

> **기반 파일:** `presentation/index.html`
> **타입:** 단일 HTML 슬라이드쇼 (Self-contained, no build tools)
> **출력 형식:** A4 가로 (landscape) — 브라우저 열람 + PDF 출력 가능

---

## 1. 파일 구조 (템플릿 골격)

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
        /* === 2. 기본 리셋 및 전역 스타일 === */
        /* === 3. 슬라이드 기본 레이아웃 (transform 기반) === */
        /* === 4. 시작 화면 (Title Slide) === */
        /* === 5. 표준 콘텐츠 슬라이드 === */
        /* === 6. 슬라이드별 특화 스타일 === */
        /* === 7. 네비게이션, PDF/PPT 출력 === */
    </style>
</head>
<body>

    <div class="slides-wrapper">
        <!-- === 슬라이드들 === -->
    </div>

    <!-- 네비게이션 + PDF/PPT 버튼 (wrapper 밖) -->
    <div class="nav-hint">← → 키로 슬라이드 이동 | Ctrl+P로 인쇄</div>
    <button class="ppt-download-btn" id="pptBtn" onclick="downloadPPT()">PPT 다운로드</button>
    <button class="pdf-download-btn" onclick="downloadPDF()">PDF 다운로드</button>

    <script>
        /* === 네비게이션 + PDF/PPT 출력 로직 === */
    </script>
</body>
</html>
```

---

## 2. 기본 리셋 및 전역 스타일 (필수)

```css
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

@page {
    size: A4 landscape;
    margin: 0;
}

body {
    font-family: 'Segoe UI', -apple-system, BlinkMacSystemFont, sans-serif;
    background: #f0f2f5;
    color: #1a1a2e;
    margin: 0;
    padding: 0;
    overflow: hidden;
}

.slides-wrapper {
    display: flex;
    transition: transform 0.4s ease;
}
```

---

## 3. 슬라이드 기본 레이아웃 (필수)

```css
.slide {
    min-width: 100vw;
    width: 100vw;
    height: 100vh;
    padding: 60px 80px;          /* ← 상하 60px, 좌우 80px (고정) */
    display: flex;
    flex-direction: column;
    justify-content: center;
    page-break-after: always;
    position: relative;
    overflow: hidden;
    flex-shrink: 0;
}

.slide-number {
    position: absolute;
    bottom: 30px;                /* ← 위치 고정 */
    right: 50px;                 /* ← 위치 고정 */
    font-size: 14px;
    color: #666;
    font-weight: 500;
}
```

> **규격:** 모든 슬라이드는 `padding: 60px 80px` 공유. `slide-featmap` 등 일부 특수 슬라이드만 예외적으로 `padding` 오버라이드 가능.

---

## 4. 표준 헤더 (타이틀 영역)

모든 콘텐츠 슬라이드는 동일한 `.slide-header` 구조를 사용:

```html
<div class="slide-header">
    <h2>슬라이드 제목</h2>
    <div class="accent-line"></div>
</div>
```

```css
.slide-header {
    margin-bottom: 50px;         /* ← 헤더와 본문 사이 간격 고정 */
}

.slide-header h2 {
    font-size: 36px;             /* ← 타이틀 폰트 사이즈 고정 */
    font-weight: 700;
    color: #1a1a2e;
    margin-bottom: 10px;
}

.slide-header .accent-line {
    width: 80px;
    height: 4px;
    background: linear-gradient(90deg, #e94560, #0f3460);
    border-radius: 2px;
}
```

> **규격:** 타이틀 위치 = `padding: 60px 80px` 기준 상단에서 `60px + 헤더 h2 높이`에 고정. 모든 페이지 통일.

---

## 5. 슬라이드 유형별 템플릿

### 5.1 Type A — 시작 화면 (Title Slide)

**CSS class:** `.slide-1`

```html
<div class="slide slide-1">
    <div class="title-logo">
        프로젝트명<span>Accent</span>
    </div>
    <div class="title-subtitle">부제목</div>
    <div class="title-meta">날짜 / 이벤트 정보</div>
    <div class="title-badge">뱃지 텍스트</div>
    <div class="slide-number">1 / N</div>
    <div class="contributors">
        <div class="contributors-label">Contributors</div>
        <a class="contributor" href="#">
            <img src="avatar.png" alt="name">
            <span class="contributor-name">name</span>
        </a>
        <!-- ... -->
    </div>
</div>
```

```css
.slide-1 {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
    color: white;
    text-align: center;
    justify-content: center;
    align-items: center;
}

/* 배경 애니메이션 (선택) */
.slide-1::before {
    content: '';
    position: absolute;
    top: -50%;
    right: -50%;
    width: 100%;
    height: 200%;
    background: radial-gradient(circle, rgba(233, 69, 96, 0.1) 0%, transparent 70%);
    animation: pulse 8s ease-in-out infinite;
}

.title-logo {
    font-size: 72px;             /* ← 메인 타이틀 */
    font-weight: 800;
    letter-spacing: -2px;
    margin-bottom: 20px;
    position: relative;
    z-index: 1;
}
.title-logo span {
    color: #e94560;              /* ← 강조 색상 (red) */
}

.title-subtitle {
    font-size: 28px;             /* ← 부제목 */
    font-weight: 300;
    opacity: 0.9;
    margin-bottom: 40px;
    position: relative;
    z-index: 1;
}

.title-meta {
    font-size: 16px;
    opacity: 0.7;
    position: relative;
    z-index: 1;
}

.title-badge {
    display: inline-block;
    background: rgba(233, 69, 96, 0.2);
    border: 1px solid rgba(233, 69, 96, 0.5);
    padding: 8px 20px;
    border-radius: 20px;
    margin-top: 30px;
    font-size: 14px;
    position: relative;
    z-index: 1;
}

.slide-1 .slide-number {
    bottom: 80px;               /* ← 시작 화면은 번호 위치 조정 */
}

.contributors {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 14px;
    margin-top: 24px;
    flex-wrap: wrap;
}
.contributor img {
    width: 34px;
    height: 34px;
    border-radius: 50%;
    border: 2px solid rgba(255,255,255,0.25);
}
.contributor-name {
    font-size: 10px;
    color: rgba(255,255,255,0.5);
}
```

---

### 5.2 Type B — 표준 콘텐츠 슬라이드 (가장 일반적)

**CSS class:** `.slide-N` (`.slide-2`, `.slide-3`, `.slide-5`~`.slide-14` 등)

```html
<div class="slide slide-N">
    <div class="slide-header">
        <h2>슬라이드 제목</h2>
        <div class="accent-line"></div>
    </div>
    <!-- 본문 영역: flex: 1; min-height: 0; 필수 -->
    <div class="content-area">
        ...
    </div>
    <div class="slide-number">N / total</div>
</div>
```

```css
.slide-N {
    background: linear-gradient(180deg, #ffffff 0%, #f8f9fa 100%);
}

/* 본문 컨테이너 공통 규칙 */
.slide-N > div:not(.slide-header):not(.slide-number) {
    flex: 1;                    /* ← 남은 공간 모두 사용 */
    min-height: 0;              /* ← flex overflow 대비 필수 */
}
```

> **규격:** 헤더 아래 본문 영역은 `flex: 1; min-height: 0`으로 슬라이드 하단까지 확장.

---

### 5.3 Type C — 종료 화면 (Summary/Closing Slide)

**CSS class:** `.slide-15`

```html
<div class="slide slide-15">
    <div class="summary-content">
        <div class="summary-left">
            <h3>총 정리</h3>
            <div class="summary-stats">
                <div class="summary-stat">
                    <div class="number">14</div>
                    <div class="label">항목</div>
                </div>
                <!-- ... -->
            </div>
        </div>
        <div class="summary-right">
            <h3>향후 계획</h3>
            <ul class="roadmap">
                <li><span class="roadmap-phase">Phase 1</span><span>내용</span></li>
                <!-- ... -->
            </ul>
        </div>
    </div>
    <div class="closing-message">
        <p>마무리 메시지</p>
    </div>
    <div class="slide-number">N / total</div>
</div>
```

```css
.slide-15 {
    background: linear-gradient(135deg, #1a1a2e 0%, #16213e 50%, #0f3460 100%);
    color: white;
}

.summary-content {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 50px;
}

.summary-left h3, .summary-right h3 {
    font-size: 28px;
    margin-bottom: 30px;
}

.summary-stats {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 20px;
}
.summary-stat {
    background: rgba(255,255,255,0.1);
    border-radius: 12px;
    padding: 20px;
    text-align: center;
}
.summary-stat .number {
    font-size: 32px;
    font-weight: 700;
    color: #e94560;
}
.summary-stat .label {
    font-size: 18px;
    font-weight: 500;
    opacity: 0.8;
    margin-top: 5px;
}

.roadmap {
    list-style: none;
}
.roadmap li {
    padding: 15px 0;
    border-bottom: 1px solid rgba(255,255,255,0.1);
    display: flex;
    align-items: center;
    gap: 15px;
}
.roadmap li span {
    font-size: 18px;
}

.closing-message {
    text-align: center;
    margin-top: 50px;
    padding-top: 30px;
    border-top: 1px solid rgba(255,255,255,0.2);
}
.closing-message p {
    font-size: 17px;
    line-height: 1.6;
    opacity: 0.9;
}
```

---

## 6. 슬라이드별 특화 레이아웃

### 6.1 개요 Grid (2×2 카드)

**사용처:** 프로젝트 개요, 4가지 핵심 가치 등

```html
<div class="slide slide-2">
    <div class="slide-header">...</div>
    <div class="overview-grid">
        <div class="overview-card">
            <div class="card-icon"><!-- SVG --></div>
            <h3>제목</h3>
            <p>설명</p>
        </div>
        <!-- x4 -->
    </div>
    <div class="stat-row">
        <div class="stat-item">
            <div class="stat-number">37</div>
            <div class="stat-label">Commits</div>
        </div>
        <!-- ... -->
    </div>
    <div class="slide-number">N / total</div>
</div>
```

```css
.overview-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    grid-template-rows: 1fr 1fr;
    gap: 20px;
    flex: 1;
    min-height: 0;
}

.overview-card {
    background: white;
    border-radius: 16px;
    padding: 28px 30px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.08);
    border-left: 4px solid;
}
.overview-card:nth-child(1) { border-color: #e94560; }
.overview-card:nth-child(2) { border-color: #0f3460; }
.overview-card:nth-child(3) { border-color: #16a085; }
.overview-card:nth-child(4) { border-color: #9b59b6; }

.overview-card h3 {
    font-size: 26px;            /* ← 카드 타이틀 */
    font-weight: 700;
    color: #1a1a2e;
    display: flex;
    align-items: center;
    gap: 10px;
}
.overview-card h3 svg {
    width: 24px;
    height: 24px;
    stroke-width: 2;
    fill: none;
    stroke: currentColor;
}
.overview-card p {
    font-size: 18px;            /* ← 카드 본문 */
    line-height: 1.6;
    color: #555;
}

.stat-number { font-size: 32px; font-weight: 700; color: #e94560; }
.stat-label  { font-size: 15px; color: #666; }
```

---

### 6.2 Feature Grid (3열 카드)

**사용처:** 기능 목록, 특징 소개

```html
<div class="slide slide-3">
    <div class="slide-header">...</div>
    <div class="feature-grid">
        <div class="feature-card">
            <div class="feature-icon"><!-- SVG --></div>
            <h4>기능명</h4>
            <p>설명</p>
        </div>
        <!-- x6 (3열 × 2행) -->
    </div>
    <div class="slide-number">N / total</div>
</div>
```

```css
.feature-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 24px;
    flex: 1;
    min-height: 0;
}

.feature-card {
    background: white;
    border-radius: 12px;
    padding: 24px;
    box-shadow: 0 2px 12px rgba(0,0,0,0.06);
}

.feature-icon {
    width: 48px;
    height: 48px;
    border-radius: 12px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 16px;
}
.feature-icon svg {
    width: 24px;
    height: 24px;
    stroke-width: 2;
    fill: none;
    stroke: currentColor;
}
.feature-card:nth-child(1) .feature-icon { background: #ffe8ec; color: #e94560; }
.feature-card:nth-child(2) .feature-icon { background: #e8f4fd; color: #0f3460; }
.feature-card:nth-child(3) .feature-icon { background: #e8f8f5; color: #16a085; }
.feature-card:nth-child(4) .feature-icon { background: #f4ecf7; color: #9b59b6; }
.feature-card:nth-child(5) .feature-icon { background: #fef5e7; color: #f39c12; }
.feature-card:nth-child(6) .feature-icon { background: #fde8e8; color: #c0392b; }

.feature-card h4 {
    font-size: 26px;            /* ← 기능명 */
    font-weight: 700;
    margin-bottom: 12px;
    color: #1a1a2e;
}
.feature-card p {
    font-size: 18px;            /* ← 설명 */
    line-height: 1.6;
    color: #555;
}
```

---

### 6.3 Feature Map (3×2 그룹 카드)

**사용처:** 전 기능 구성도, 전체 메뉴 구조

```html
<div class="slide slide-featmap">
    <div class="slide-header">...</div>
    <div class="feat-grid">
        <div class="feat-group feat-group-1">
            <div class="feat-header">
                <svg><!-- icon --></svg>
                그룹명
                <span class="badge">뱃지</span>
            </div>
            <div class="feat-body">
                <div class="feat-item"><strong>항목:</strong> 값</div>
                <!-- x6 -->
                <div class="feat-more">+ 추가 정보</div>
            </div>
        </div>
        <!-- x6 (3열 × 2행) -->
    </div>
    <div class="slide-number">N / total</div>
</div>
```

```css
.feat-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    flex: 1;
    min-height: 0;
    align-content: start;
}

.feat-group {
    background: white;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.06);
    overflow: hidden;
    display: flex;
    flex-direction: column;
}

.feat-header {
    display: flex;
    align-items: center;
    gap: 10px;
    padding: 12px 16px;
    font-weight: 700;
    font-size: 17px;
    letter-spacing: -0.3px;
    border-bottom: 1px solid #f0f0f0;
}
.feat-header .badge {
    margin-left: auto;
    font-size: 12px;
    font-weight: 600;
    padding: 2px 10px;
    border-radius: 10px;
    background: #f0f0f0;
    color: #666;
}

.feat-body {
    padding: 10px 16px 12px;
    flex: 1;
}

.feat-item {
    font-size: 14.5px;
    line-height: 1.5;
    color: #444;
    padding: 3px 0;
    display: flex;
    align-items: center;
    gap: 6px;
}
.feat-item::before {
    content: '';
    width: 4px;
    height: 4px;
    border-radius: 50%;
    flex-shrink: 0;
    background: #ccc;
}
.feat-item strong {
    color: #1a1a2e;
    font-weight: 600;
}

.feat-more {
    font-size: 12px;
    color: #999;
    padding: 6px 0 2px;
    font-style: italic;
}

/* 6개 그룹 헤더 색상 */
.feat-group-1 .feat-header { background: linear-gradient(135deg, #0f3460, #1a5276); color: white; }
.feat-group-2 .feat-header { background: linear-gradient(135deg, #e94560, #c0392b); color: white; }
.feat-group-3 .feat-header { background: linear-gradient(135deg, #16a085, #1abc9c); color: white; }
.feat-group-4 .feat-header { background: linear-gradient(135deg, #9b59b6, #8e44ad); color: white; }
.feat-group-5 .feat-header { background: linear-gradient(135deg, #f39c12, #e67e22); color: white; }
.feat-group-6 .feat-header { background: linear-gradient(135deg, #c0392b, #96281b); color: white; }

.feat-group-N .feat-header .badge { background: rgba(255,255,255,0.2); color: rgba(255,255,255,0.9); }
```

---

### 6.4 스크린샷 (전폭 이미지)

**사용처:** 화면 캡처, 이미지 위주의 슬라이드

```html
<div class="slide slide-5">
    <div class="slide-header">...</div>
    <div class="cli-screenshot">
        <img src="image.png" alt="설명">
    </div>
    <div class="slide-number">N / total</div>
</div>
```

```css
.cli-screenshot {
    flex: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    min-height: 0;
    padding: 0 20px;
}
.cli-screenshot img {
    max-width: 100%;
    max-height: 100%;
    object-fit: contain;
    border-radius: 12px;
    box-shadow: 0 4px 24px rgba(0,0,0,0.12);
}
```

---

### 6.5 API Spec Grid (3열)

```css
.apispec-grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 16px;
    flex: 1;
    min-height: 0;
    align-content: start;
}

.apispec-group {
    background: white;
    border-radius: 10px;
    padding: 14px 16px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.06);
    border: 1px solid rgba(0,0,0,0.05);
}

.apispec-header {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 10px;
    padding-bottom: 8px;
    border-bottom: 1px solid #f0f0f0;
}

.apispec-icon {
    width: 22px; height: 22px;
    border-radius: 6px;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 11px;
    font-weight: 800;
    flex-shrink: 0;
}

.apispec-title { font-size: 13px; font-weight: 700; color: #1a1a2e; flex: 1; }
.apispec-count { font-size: 10px; font-weight: 700; color: #999; }

.apispec-item {
    font-size: 10px;
    color: #555;
    padding: 3px 0;
    font-family: 'Monaco', 'Menlo', monospace;
    line-height: 1.5;
}
```

---

### 6.6 블록 다이어그램

**사용처:** 복잡한 UI 레이아웃/아키텍처 시각화

```css
.block-diagram {
    display: flex;
    flex-direction: column;
    gap: 12px;
    flex: 1;
    min-height: 0;
}

.block-main {
    display: grid;
    grid-template-columns: 200px 1fr 260px;
    gap: 12px;
    flex: 1;
    min-height: 0;
}
```

상세 구조는 `presentation/index.html`의 `.slide-4` (블록 다이어그램) 참조.

---

### 6.7 CLI 명령어 테이블

```css
.cli-layout {
    display: grid;
    grid-template-columns: 1fr 1.4fr;
    gap: 30px;
    flex: 1;
    min-height: 0;
}

.cli-cmd-table td {
    padding: 4px 8px;
    font-size: 11px;
    border-bottom: 1px solid #f5f5f5;
}
.cli-cmd-table td:first-child {
    font-family: 'Monaco', 'Menlo', monospace;
    color: #e94560;
    font-weight: 600;
    width: 110px;
}
```

---

### 6.8 분할 레이아웃 (2열)

```css
.split-layout {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 40px;
    flex: 1;
    min-height: 0;
}

.split-section {
    background: white;
    border-radius: 16px;
    padding: 30px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.08);
}
.split-section h3 {
    font-size: 22px;
    margin-bottom: 20px;
    padding-bottom: 15px;
    border-bottom: 2px solid #f0f0f0;
}
```

---

### 6.9 기술 상세 Grid (2×2)

```css
.tech-grid {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    gap: 24px;
    flex: 1;
    min-height: 0;
}

.tech-card {
    background: white;
    border-radius: 16px;
    padding: 30px;
    box-shadow: 0 4px 20px rgba(0,0,0,0.08);
}
.tech-card h4 {
    font-size: 26px;
    font-weight: 700;
    margin-bottom: 12px;
    color: #1a1a2e;
}
.tech-card p {
    font-size: 18px;
    line-height: 1.6;
    color: #555;
}
```

---

### 6.10 LLM Provider 테이블

```css
.llm-table {
    width: 100%;
    border-collapse: collapse;
    font-size: 15px;
}

.llm-table thead th {
    background: linear-gradient(135deg, #1a1a2e, #0f3460);
    color: white;
    padding: 14px 18px;
    text-align: left;
    font-weight: 700;
    font-size: 14px;
    border-bottom: 2px solid #e94560;
}

.llm-table tbody td {
    padding: 12px 18px;
    border-bottom: 1px solid #f0f0f0;
    color: #444;
}

.llm-badge.yes  { background: #e8f8f5; color: #16a085; }
.llm-badge.no   { background: #f5f5f5; color: #999; }
.llm-badge.shim { background: #fef5e7; color: #f39c12; }
.llm-badge.cloud { background: #e8f4fd; color: #0f3460; }
.llm-badge.local { background: #f4ecf7; color: #9b59b6; }
```

---

## 7. 네비게이션, PDF/PPT 출력

### 외부 라이브러리 (CDN)

PPT 다운로드 기능을 위해 `<head>`에 다음 라이브러리를 추가합니다.

```html
<script src="https://cdn.jsdelivr.net/npm/html2canvas@1.4.1/dist/html2canvas.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/pptxgenjs@3.12.0/dist/pptxgen.bundle.js"></script>
```

- **html2canvas** — DOM 요소를 캔버스로 캡처 (스크린샷)
- **pptxgenjs** — 브라우저에서 PPTX 파일 생성

### HTML (슬라이드 이후, body 닫기 전)

`nav-hint`, `ppt-download-btn`, `pdf-download-btn`은 `.slides-wrapper` **바깥**에 위치해야 합니다.
그래야 transform 기반 네비게이션과 스크린샷 캡처 시 간섭이 없습니다.

```html
<div class="nav-hint">← → 키로 슬라이드 이동 | Ctrl+P로 인쇄</div>

<button class="ppt-download-btn" id="pptBtn" onclick="downloadPPT()">
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
        <polyline points="14 2 14 8 20 8"/>
        <rect x="8" y="12" width="8" height="6" rx="1"/>
    </svg>
    PPT 다운로드
</button>

<button class="pdf-download-btn" onclick="downloadPDF()">
    <svg width="18" height="18" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2">
        <path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8z"/>
        <polyline points="14 2 14 8 20 8"/>
        <line x1="16" y1="13" x2="8" y2="13"/>
        <line x1="16" y1="17" x2="8" y2="17"/>
    </svg>
    PDF 다운로드
</button>
```

### CSS

```css
.nav-hint {
    position: fixed;
    bottom: 20px;
    left: 50%;
    transform: translateX(-50%);
    background: rgba(0,0,0,0.7);
    color: white;
    padding: 8px 16px;
    border-radius: 20px;
    font-size: 12px;
    opacity: 0.7;
    z-index: 1000;
    pointer-events: none;
}

.ppt-download-btn {
    position: fixed;
    top: 24px;
    right: 190px;
    z-index: 1001;
    background: #0f3460;
    color: white;
    border: none;
    padding: 10px 22px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: inherit;
}
.ppt-download-btn:disabled {
    opacity: 0.6;
    cursor: not-allowed;
}

.pdf-download-btn {
    position: fixed;
    top: 24px;
    right: 30px;
    z-index: 1001;
    background: #e94560;
    color: white;
    border: none;
    padding: 10px 22px;
    border-radius: 8px;
    cursor: pointer;
    font-size: 14px;
    font-weight: 600;
    display: flex;
    align-items: center;
    gap: 8px;
    font-family: inherit;
}

@media print {
    body {
        overflow: visible;
        display: block;
    }
    .slides-wrapper {
        transform: none !important;
        display: block;
        width: auto;
    }
    .slide {
        min-width: 0;
        width: 100%;
        height: 100vh;
        page-break-after: always;
        page-break-inside: avoid;
        flex-shrink: 0;
        -webkit-print-color-adjust: exact;
        print-color-adjust: exact;
    }
    .nav-hint, .pdf-download-btn, .ppt-download-btn { display: none; }
}
```

### JavaScript

```javascript
(function() {
    var current = 0;
    var slides = document.querySelectorAll('.slide');
    var total = slides.length;
    var wrapper = document.querySelector('.slides-wrapper');
    var locked = false;

    function goTo(idx) {
        if (idx < 0 || idx >= total || locked) return;
        locked = true;
        current = idx;
        wrapper.style.transform = 'translateX(' + (-current * 100) + 'vw)';
        setTimeout(function() { locked = false; }, 450);
    }

    document.addEventListener('keydown', function(e) {
        if (e.key === 'ArrowRight' || e.key === 'ArrowDown') {
            e.preventDefault();
            goTo(current + 1);
        } else if (e.key === 'ArrowLeft' || e.key === 'ArrowUp') {
            e.preventDefault();
            goTo(current - 1);
        }
    });
})();

function downloadPDF() {
    window.print();
}

function downloadPPT() {
    var btn = document.getElementById('pptBtn');
    btn.disabled = true;
    btn.textContent = '생성 중...';

    var slides = document.querySelectorAll('.slide');
    var wrapper = document.querySelector('.slides-wrapper');
    var savedTransform = wrapper.style.transform;
    wrapper.style.transition = 'none';

    var images = [];
    var index = 0;

    function captureNext() {
        if (index >= slides.length) {
            wrapper.style.transform = savedTransform;
            wrapper.style.transition = 'transform 0.4s ease';
            buildPPTX(images);
            return;
        }
        wrapper.style.transform = 'translateX(' + (-index * 100) + 'vw)';
        setTimeout(function() {
            html2canvas(slides[index], {
                scale: 2, useCORS: true, backgroundColor: null, logging: false
            }).then(function(canvas) {
                images.push(canvas.toDataURL('image/png'));
                index++;
                captureNext();
            });
        }, 100);
    }
    captureNext();
}

function buildPPTX(images) {
    var pptx = new PptxGenJS();
    pptx.defineLayout({ name: 'CUSTOM', width: 13.333, height: 7.5 });
    pptx.layout = 'CUSTOM';

    images.forEach(function(dataUrl) {
        var slide = pptx.addSlide();
        slide.addImage({ data: dataUrl, x: 0, y: 0, w: 13.333, h: 7.5 });
    });

    pptx.writeFile({ fileName: 'litellm-intro.pptx' }).then(function() {
        var btn = document.getElementById('pptBtn');
        btn.disabled = false;
        btn.innerHTML = '<svg ...>...</svg> PPT 다운로드';
    });
}
```

### PPT 생성 동작 원리

1. `downloadPPT()` 호출 시 wrapper의 `transform`을 일시 정지
2. 각 슬라이드를 순차적으로 화면에 표시한 후 `html2canvas(scale:2)`로 캡처
3. 캡처 이미지에는 `nav-hint`, `pdf-download-btn`, `ppt-download-btn`이 포함되지 않음 (`position: fixed`로 슬라이드 영역 밖)
4. 모든 슬라이드 캡처 완료 후 `pptxgenjs`로 PPTX 파일 생성 및 자동 다운로드
5. 레이아웃: A4 가로 (13.333 × 7.5 inch), 각 캡처 이미지를 슬라이드 전체에 배치

---

## 8. 규격 요약표

| 항목 | 값 | 비고 |
|------|-----|------|
| 페이지 크기 | A4 landscape (297×210mm) | `@page { size: A4 landscape; margin: 0; }` |
| 슬라이드 패딩 | `60px 80px` | 상하 60px, 좌우 80px |
| 헤더 하단 마진 | `50px` | 타이틀과 본문 사이 |
| 타이틀 폰트 | `36px` / 700 weight | `h2` 기준 |
| 부제목 폰트 | `28px` / 300 weight | 시작 화면 `title-subtitle` |
| 카드 타이틀 폰트 | `26px` | `h3`, `h4`, `.tech-card h4` |
| 카드 본문 폰트 | `18px` | `p`, `.feature-card p` |
| 슬라이드 번호 | `14px` / 우하단 고정 | `bottom: 30px; right: 50px` |
| 강조 색상 | `#e94560` (Red) | 악센트 라인, 포인트 |
| 배경색 | `#ffffff → #f8f9fa` | 콘텐츠 슬라이드 |
| 시작화면 배경 | `#1a1a2e → #16213e → #0f3460` | 다크 그라데이션 |
| 종료화면 배경 | `#1a1a2e → #16213e → #0f3460` | 시작화면과 동일 |
| 카드 그림자 | `0 4px 20px rgba(0,0,0,0.08)` | 기본 카드 |
| 작은 카드 그림자 | `0 2px 12px rgba(0,0,0,0.06)` | feature, feat-group |
| 여백 색상 | `#f0f2f5` | body background |
| 본문 글자색 | `#555` / `#444` | 일반 텍스트 |
| 타이틀 글자색 | `#1a1a2e` | 다크 네이비 |

---

## 9. 슬라이드 생성 워크플로우

```
1. HTML 파일 생성 → <head>에 CDN 라이브러리 + 기본 리셋 + 전역 스타일 복사
2. <body> 안에 .slides-wrapper 래퍼 생성
3. 시작 화면 (.slide-1) 추가
4. 콘텐츠 슬라이드 추가 (필요한 Type B~E 템플릿 선택)
5. 종료 화면 (.slide-15) 추가
6. .slides-wrapper 닫기
7. wrapper 밖에 nav-hint + ppt-download-btn + pdf-download-btn 추가
8. 각 슬라이드에 .slide-number 추가 (N / total 형식)
9. 네비게이션 + PDF/PPT 출력 JS 복사
10. 브라우저에서 열어 검증
11. Ctrl+P → PDF 저장 또는 PPT 다운로드 버튼 클릭
```

### CSS 클래스 네이밍 규칙

| 클래스 | 용도 |
|--------|------|
| `.slide-1` | 시작 화면 (Title) |
| `.slide-2` | 개요 Grid (2×2) |
| `.slide-3` | Feature Grid (3열) |
| `.slide-4` | 블록 다이어그램 |
| `.slide-5` ~ `.slide-8` | 스크린샷 (전폭 이미지) |
| `.slide-9` | CLI 명령어 |
| `.slide-10` | CLI 스크린샷 |
| `.slide-11` | 분할 레이아웃 (2열) |
| `.slide-12` ~ `.slide-13` | 스크린샷/기술 |
| `.slide-14` | Direction |
| `.slide-15` | 종료 화면 (Summary) |
| `.slide-featmap` | 기능 구성도 (3×2 그룹) |
| `.slide-arch` | 아키텍처 다이어그램 |
| `.slide-context` | 컨텍스트 수집 흐름 |
| `.slide-llm` | LLM Provider 테이블 |
| `.slide-cli-ref` | CLI 참고 이미지 |

