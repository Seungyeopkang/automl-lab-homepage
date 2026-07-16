# AutoML Lab 홈페이지 구현 계획

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 경기대 AutoML Lab의 fancy 원페이지 정적 홈페이지 — 학생들이 `data/*.yml`만 수정해 콘텐츠를 업데이트.

**Architecture:** 빌드 없는 순수 정적 사이트. `index.html` 뼈대 + `js/main.js`가 js-yaml(CDN)로 `data/*.yml` 4개를 fetch·파싱·렌더링. 다크 테마 + 신경망 파티클 캔버스 + 스크롤 애니메이션. 로컬/GitHub Pages/서버컴 동일 파일.

**Tech Stack:** HTML/CSS/Vanilla JS, js-yaml 4.x (CDN), Pretendard 폰트 (CDN), Python http.server (로컬 테스트), Playwright MCP (검증).

## Global Constraints

- 빌드 단계 금지 — 파일 수정 즉시 반영되어야 함
- 외부 의존성은 js-yaml CDN + Pretendard CDN 두 개만
- 국문 위주 UI (논문 제목·연구실명 영문 유지)
- YAML 오류 시 해당 섹션만 오류 표시, 나머지 정상 렌더링
- `prefers-reduced-motion` 존중, 모바일(≥360px) 완전 대응
- 스펙: `docs/superpowers/specs/2026-07-16-automl-lab-homepage-design.md`

---

### Task 1: 데이터 파일 4종 (CV 실데이터)

**Files:**
- Create: `data/members.yml`, `data/publications.yml`, `data/projects.yml`, `data/news.yml`
- Create: `images/members/.gitkeep`

**Interfaces:**
- Produces: Task 3의 렌더러가 소비하는 YAML 스키마 (스펙 §데이터 스키마와 동일). 필드명 정확히: members(`name, name_en, role, photo, interests, email, github, joined`), publications(`title, authors, venue, venue_full, year, type, highlight, link`), projects(`title, period, funder, description, status`), news(`date, category, text`)

- [ ] **Step 1: `data/members.yml` 작성**

```yaml
# 연구실 멤버 — 새 멤버는 아래 형식 복사해서 추가
# role: 교수 | 박사과정 | 석사과정 | 학부연구생 | 졸업생
- name: 서왕덕
  name_en: Wangduk Seo
  role: 교수
  photo: images/members/wdseo.jpg
  interests: [Neural Architecture Search, Efficient Deep Learning, Feature Selection, Evolutionary Algorithm]
  email: wdseo@kyonggi.ac.kr
  joined: 2024-09
```

- [ ] **Step 2: `data/publications.yml` 작성 — CV_korean.tex 전체 실적 옮김**

```yaml
# 논문/특허 — type: journal | conference | domestic | patent
# highlight: true 붙이면 대표 실적 배지
- title: "Automatic Background Animation Generation Aligned with LLM-generated Lyrics for Children's Songs"
  authors: "Sanghyuck Lee, Timur Khairulov, Ye-Chan Park, Wangduk Seo, Jaesung Lee"
  venue: Scientific Reports
  year: 2026
  type: journal
- title: "NA-Search: Differentiable Search of Normalization and Activation"
  authors: "Wangduk Seo"
  venue: 한국컴퓨터정보학회논문지
  year: 2026
  type: domestic
- title: "ChainImputer: A Neural Network-Based Iterative Imputation Method Using Cumulative Features"
  authors: "Wangduk Seo, Timur Khairulov, Hye-Jin Baek, Jaesung Lee"
  venue: Symmetry
  year: 2025
  type: journal
- title: "Unsupervised Feature Selection towards Pattern Discrimination Power"
  authors: "Wangduk Seo, Jaesung Lee"
  venue: UAI
  venue_full: "The 40th Conference on Uncertainty in Artificial Intelligence"
  year: 2024
  type: conference
  highlight: true
- title: "Memetic Multilabel Feature Selection using Pruned Refinement Process"
  authors: "Wangduk Seo, Jaegyun Park, Sanghyuck Lee, A-Seong Moon, Dae-Won Kim, Jaesung Lee"
  venue: Journal of Big Data
  year: 2024
  type: journal
  highlight: true
- title: "A Short Survey and Comparison of CNN-based Music Genre Classification using Multiple Spectral Features"
  authors: "Wangduk Seo, Sung-Hyun Cho, Pawel Teisseyre, Jaesung Lee"
  venue: IEEE Access
  year: 2023
  type: journal
- title: "Efficient Human Activity Recognition using Lookup Table-based Neural Architecture Search for Mobile Devices"
  authors: "Won-Seon Lim, Wangduk Seo, Dae-Won Kim, Jaesung Lee"
  venue: IEEE Access
  year: 2023
  type: journal
- title: "Achieving an Optimal Group Structure in a Neural Architecture Search"
  authors: "Wangduk Seo, Jaegyun Park, Won-Seon Lim, Dae-Won Kim, Jaesung Lee"
  venue: Electronics Letters
  year: 2022
  type: journal
- title: "Effective Memetic Algorithm for Multilabel Feature Selection using Hybridization-based Communication"
  authors: "Wangduk Seo, Minwoo Park, Dae-Won Kim, Jaesung Lee"
  venue: Expert Systems with Applications
  year: 2022
  type: journal
  highlight: true
- title: "Neural Network-based Method for Diagnosis and Severity Assessment of Graves' Orbitopathy using Orbital Computed Tomography"
  authors: "Jaesung Lee, Wangduk Seo, Jaegyun Park, Won-Seon Lim, Ja Young Oh, Nam Ju Moon, Jeong Kyu Lee"
  venue: Scientific Reports
  year: 2022
  type: journal
- title: "Generalized Information-theoretic Criterion for Multi-label Feature Selection"
  authors: "Wangduk Seo, Dae-Won Kim, Jaesung Lee"
  venue: IEEE Access
  year: 2019
  type: journal
- title: "Compact feature subset-based multi-label music categorization for mobile devices"
  authors: "Jaesung Lee, Wangduk Seo, Jin-Hyeong Park, Dae-Won Kim"
  venue: Multimedia Tools and Applications
  year: 2019
  type: journal
- title: "Effective Evolutionary Multilabel Feature Selection under a Budget Constraint"
  authors: "Jaesung Lee, Wangduk Seo, Dae-Won Kim"
  venue: Complexity
  year: 2018
  type: journal
- title: "Evolutionary Multilabel Feature Selection Using Promising Feature Subset Generation"
  authors: "Jaesung Lee, Wangduk Seo, Ho Han, Dae-Won Kim"
  venue: Journal of Sensors
  year: 2018
  type: journal
- title: "Explosive Reproduction-based Memetic Search for Effective Multi-label Feature Selection"
  authors: "Wangduk Seo, Ho Han, Dae-Won Kim, Jaesung Lee"
  venue: ICASI
  venue_full: "International Conference on Applied System Innovation (구두발표, Best Paper Award)"
  year: 2018
  type: conference
- title: "Toward Accurate Music Classification using Local Set-based Multi-label Prototype Selection"
  authors: "Wangduk Seo, Sanghyun Seo, Sang Oh Park, Mucheol Kim, Jaesung Lee"
  venue: ICNCT
  venue_full: "International Conference on Next-generation Convergence Technology (구두발표, Best Paper Award)"
  year: 2018
  type: conference
- title: "Effective Multi-label Feature Selection based on Large Offspring Set created by Enhanced Evolutionary Search Process"
  authors: "Hyunki Lim, Wangduk Seo, Jaesung Lee"
  venue: 한국컴퓨터정보학회논문지
  year: 2018
  type: domestic
- title: "Efficient Information-theoretic Unsupervised Feature Selection"
  authors: "Jaesung Lee, Wangduk Seo, Dae-Won Kim"
  venue: Electronics Letters
  year: 2017
  type: journal
- title: "비지도 학습 기반 특징 선택 방법 및 장치 (제10-2092026호)"
  authors: "서왕덕 외"
  venue: 대한민국 특허 등록
  year: 2020
  type: patent
- title: "다중 레이블 패턴 분류를 위한 특징 하위 집합 생성 방법 및 그 장치 (제10-2066157호)"
  authors: "서왕덕 외"
  venue: 대한민국 특허 등록
  year: 2020
  type: patent
- title: "다중 레이블 패턴 분류를 위한 특징 선택 방법 및 그 장치 (제10-2025280호)"
  authors: "서왕덕 외"
  venue: 대한민국 특허 등록
  year: 2019
  type: patent
```

- [ ] **Step 3: `data/projects.yml` 작성**

```yaml
# 연구과제 — status: 진행중 | 완료
- title: "Deep Learning Framework for On-Device AI"
  period: 2019.03 – 2022
  funder: 한국연구재단
  description: 온디바이스 AI를 위한 신경망 구조 탐색 기법 개발.
  status: 완료
- title: "Korean Music Reproducing System based on Cultural Aesthetics"
  period: 2019.03 – 2022
  funder: 한국연구재단
  description: 음악 스타일 자동 재편곡 시스템 개발.
  status: 완료
- title: "Development of Computer based Three-Dimensional Medical Image Analysis Program for the Objective Assessment of Orbital Disease"
  period: 2017.06 – 2022
  funder: 한국연구재단
  description: 안와 질환 진단을 위한 딥러닝 모델 개발 (1·2차 연속 과제).
  status: 완료
- title: "Development of Intelligent Healthcare Platform for Atopic Treatment using Bio-Clinic Convergence Information"
  period: 2019.03 – 2022
  funder: 한국연구재단
  description: 의료 데이터셋을 위한 머신러닝 프레임워크 개발.
  status: 완료
```

- [ ] **Step 4: `data/news.yml` 작성**

```yaml
# 최신 소식 — 위에 추가 (최신순). category: paper | award | member | etc
- date: 2026-07
  category: etc
  text: "AutoML Lab 홈페이지 오픈 🎉"
- date: 2026-01
  category: paper
  text: "Scientific Reports 논문 게재 — LLM 기반 동요 배경 애니메이션 자동 생성"
- date: 2026-01
  category: paper
  text: "한국컴퓨터정보학회논문지 게재 — NA-Search"
- date: 2025-06
  category: paper
  text: "Symmetry 논문 게재 — ChainImputer"
- date: 2024-09
  category: member
  text: "서왕덕 교수, 경기대학교 AI컴퓨터공학부 조교수 부임 — AutoML Lab 시작"
- date: 2024-07
  category: paper
  text: "UAI 2024 논문 발표 — Unsupervised Feature Selection"
```

- [ ] **Step 5: 커밋**

```bash
git add data/ images/
git commit -m "feat: add lab data files (members, publications, projects, news)"
```

---

### Task 2: index.html 뼈대

**Files:**
- Create: `index.html`

**Interfaces:**
- Produces: 섹션 컨테이너 id — `#hero`, `#research`, `#professor`, `#members`, `#publications`, `#projects`, `#news`, `#join`. 동적 렌더 대상 id — `#members-grid`, `#pub-list`, `#pub-filters`, `#projects-grid`, `#news-timeline`, `#stat-*`. Task 3 JS와 Task 4 CSS가 이 id/class에 의존
- Consumes: `data/*.yml` (Task 1)

- [ ] **Step 1: index.html 작성**

구성 (완전한 마크업, 핵심 구조):

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AutoML Lab — 경기대학교</title>
  <meta name="description" content="경기대학교 AI컴퓨터공학부 AutoML Lab. Neural Architecture Search, 효율적 딥러닝, 특징 선택, 진화 알고리즘 연구.">
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable-dynamic-subset.min.css">
  <link rel="stylesheet" href="css/style.css">
</head>
<body>
  <canvas id="neural-bg"></canvas>
  <nav id="navbar"><!-- 로고 + 섹션 앵커 링크 + 모바일 햄버거 --></nav>
  <div id="scroll-progress"></div>
  <header id="hero">
    <h1><!-- 그라디언트 텍스트: AutoML Lab --></h1>
    <p class="hero-tagline">스스로 설계하는 AI를 만듭니다</p>
    <p class="hero-typing">우리는 <span id="typing-target"></span>를 연구합니다</p>
    <div class="hero-stats"><!-- 카운트업: 국제저널 13 · 국제학회 3 · 특허 3 · Best Paper 2 --></div>
    <a href="#join" class="cta-button">연구실 지원하기</a>
  </header>
  <main>
    <section id="research"><div class="research-grid"><!-- 정적 4카드: NAS/효율적DL/특징선택/진화알고리즘, 각 카드에 학부생 눈높이 한줄 --></div></section>
    <section id="professor"><!-- 정적: CV 기반 약력/학력/경력/수상 --></section>
    <section id="members"><div id="members-grid"></div></section>
    <section id="publications"><div id="pub-filters"></div><div id="pub-list"></div></section>
    <section id="projects"><div id="projects-grid"></div></section>
    <section id="news"><div id="news-timeline"></div></section>
    <section id="join"><!-- 학부연구생/석사/박사 3열 카드 + 지원방법 + 연락처 --></section>
  </main>
  <footer><!-- 주소, 이메일, 전화 (CV 연락처) --></footer>
  <script src="https://cdn.jsdelivr.net/npm/js-yaml@4.1.0/dist/js-yaml.min.js"></script>
  <script src="js/main.js"></script>
</body>
</html>
```

Research 4카드 문구 (학부생 눈높이):
- **Neural Architecture Search** — "신경망 설계를 사람이 아닌 AI가 하도록 만듭니다. 밤새 하이퍼파라미터 튜닝? 이제 알고리즘이 합니다."
- **Efficient Deep Learning** — "거대한 모델을 스마트폰에서도 돌아가게. 작지만 강한 모델을 만드는 기술."
- **Feature Selection** — "수천 개 변수 중 진짜 중요한 것만 골라내기. 데이터의 본질을 찾는 연구."
- **Evolutionary Algorithm** — "생물 진화의 원리로 최적해를 탐색. 자연에서 배운 알고리즘."

Join Us 3열: 학부연구생(연구 맛보기, 논문 리딩, 소규모 프로젝트) / 석사(본격 연구, 국제 논문 목표) / 박사(독립 연구자로 성장). 지원: wdseo@kyonggi.ac.kr 메일 (이름/학번/관심분야).

- [ ] **Step 2: 로컬 서버 확인**

```bash
cd /Users/wd_seo/Desktop/vsproject/homepage && python3 -m http.server 8000
curl -s -o /dev/null -w "%{http_code}" http://localhost:8000/  # 기대: 200
```

- [ ] **Step 3: 커밋** — `git add index.html && git commit -m "feat: add page skeleton"`

---

### Task 3: js/main.js — 데이터 로드 + 렌더링

**Files:**
- Create: `js/main.js`

**Interfaces:**
- Consumes: Task 1 YAML 스키마, Task 2 컨테이너 id
- Produces: `loadYaml(path)`, `renderMembers(list)`, `renderPublications(list)`, `renderProjects(list)`, `renderNews(list)`, `showDataError(sectionId, err)` — Task 5 애니메이션 코드와 같은 파일에 공존

핵심 로직 (완전 구현은 실행 시):

```js
async function loadYaml(path) {
  const res = await fetch(path + '?v=' + Date.now());   // 캐시 무효화
  if (!res.ok) throw new Error(`${path} 로드 실패 (${res.status})`);
  return jsyaml.load(await res.text());                  // 파싱 실패 시 YAMLException(mark.line 포함)
}

// 각 섹션 독립 try/catch — 한 파일 오류가 다른 섹션 못 죽임
const sections = [
  ['data/members.yml', renderMembers, 'members'],
  ['data/publications.yml', renderPublications, 'publications'],
  ['data/projects.yml', renderProjects, 'projects'],
  ['data/news.yml', renderNews, 'news'],
];
for (const [path, render, id] of sections) {
  loadYaml(path).then(render).catch(err => showDataError(id, err, path));
}
```

렌더링 규칙:
- **Members**: role 순 정렬(교수→박사→석사→학부연구생→졸업생). photo 없거나 404 → 이니셜 아바타(이름 첫 글자, 그라디언트 배경). 필수 필드(name, role) 누락 항목은 skip + `console.warn`. 마지막에 "YOU?" 모집 카드 항상 추가(#join 앵커)
- **Publications**: year 내림차순 → 연도 헤더 그룹. 필터 버튼: 전체/저널/학회/국내/특허(type 매핑). `highlight: true` → ★ 대표 배지. authors에서 랩 멤버 이름(members.yml의 name_en + name) 자동 `<strong>` 처리. venue는 컬러 배지
- **Projects**: 카드. status "진행중" → 펄스 도트 배지
- **News**: date 내림차순 타임라인. category별 아이콘 (paper 📄 / award 🏆 / member 👋 / etc 📢)
- **showDataError**: 섹션 안에 경고 박스 — "`data/xxx.yml` 오류: {메시지}. {line+1}번째 줄 근처를 확인하세요. UPDATE_GUIDE.md 참고"
- Hero 통계: publications 로드 후 계산 (journal/conference/patent 카운트 + Best Paper 2 고정) → Task 5 카운트업 트리거

- [ ] **Step 1: main.js 작성** (위 규칙 전부)
- [ ] **Step 2: 브라우저 확인** — 8개 섹션 데이터 렌더, 콘솔 에러 0
- [ ] **Step 3: 오류 시나리오** — members.yml에 탭 문자 삽입 → members 섹션만 오류 박스, 나머지 정상 → 원복
- [ ] **Step 4: 커밋** — `git commit -m "feat: add YAML data loading and rendering"`

---

### Task 4: css/style.css — Fancy 다크 테마

**Files:**
- Create: `css/style.css`

**Interfaces:**
- Consumes: Task 2 마크업, Task 3 동적 생성 class (`member-card`, `pub-item`, `pub-year-header`, `filter-btn`, `project-card`, `news-item`, `data-error` 등)

**실행 시 frontend-design 스킬 참조 (별도 스킬 호출 아닌 원칙 적용).** 디자인 토큰:

```css
:root {
  --bg: #06080f;            /* 딥 네이비 블랙 */
  --bg-card: rgba(255,255,255,0.04);
  --accent: #4f8cff;        /* 일렉트릭 블루 */
  --accent2: #a855f7;       /* 바이올렛 */
  --gradient: linear-gradient(135deg, #4f8cff, #a855f7);
  --text: #e8ecf4;
  --text-dim: #8b93a7;
  --font: 'Pretendard Variable', Pretendard, sans-serif;
}
```

요구사항:
- Hero 헤드라인: 그라디언트 텍스트(`background-clip: text`), clamp() 반응형 크기
- 내비: 글래스모피즘(`backdrop-filter: blur`), 스크롤 시 배경 진해짐, 상단 스크롤 진행바(그라디언트)
- 카드: 그라디언트 보더 글로우(hover), `translateY(-4px)` 리프트, `transition 0.3s`
- 섹션 리빌: `.reveal` 초기 `opacity:0; translateY(24px)` → `.visible` 전환 (JS IntersectionObserver)
- venue 배지: type별 색 (journal 블루/conference 바이올렛/domestic 그린/patent 앰버)
- 타임라인: 좌측 그라디언트 라인 + 도트
- 반응형: 768px 이하 그리드 1열, 햄버거 메뉴
- `@media (prefers-reduced-motion: reduce)`: 애니메이션 전부 해제

- [ ] **Step 1: style.css 작성**
- [ ] **Step 2: 데스크톱(1440px)/모바일(390px) 스크린샷 확인**
- [ ] **Step 3: 커밋** — `git commit -m "feat: add fancy dark theme styling"`

---

### Task 5: 인터랙션 — 파티클 캔버스 + 애니메이션

**Files:**
- Modify: `js/main.js` (append)

**Interfaces:**
- Consumes: `#neural-bg` 캔버스, `.hero-stats` 카운트업 대상, `.reveal` 클래스

요구사항 (전부 vanilla JS):
- **신경망 파티클**: `#neural-bg` 고정 배경. 노드 ~60개(모바일 ~30) 부유, 거리 < 140px 노드끼리 라인(거리 반비례 투명도), 마우스 반경 180px 노드 밀어내기. `requestAnimationFrame`, 탭 비활성 시 정지, `prefers-reduced-motion` 시 정적 렌더 1회
- **타이핑 로테이션**: `#typing-target`에 ["신경망 구조 탐색", "효율적 딥러닝", "특징 선택", "진화 알고리즘"] 타이핑→삭제 순환
- **카운트업**: IntersectionObserver 진입 시 0→목표값 (ease-out, 1.2s)
- **섹션 리빌**: `.reveal` 요소 관찰 → `.visible` 부여
- **스크롤 진행바** + 내비 활성 섹션 하이라이트

- [ ] **Step 1: 구현**
- [ ] **Step 2: 브라우저 확인** — 파티클 마우스 반응, 카운트업, 리빌 동작
- [ ] **Step 3: 커밋** — `git commit -m "feat: add neural particle canvas and scroll animations"`

---

### Task 6: 문서 — UPDATE_GUIDE.md + DEPLOY.md

**Files:**
- Create: `UPDATE_GUIDE.md`, `DEPLOY.md`, `README.md`

**UPDATE_GUIDE.md** (학생 대상, git 몰라도 되게):
1. "여러분이 수정할 곳은 `data/` 폴더 4개 파일뿐"
2. GitHub 웹 편집 절차 스크린샷 수준 설명 (파일 열기→연필→수정→Commit changes)
3. 파일별 형식 + 복붙용 예시 (신규 멤버/논문/과제/뉴스 각 1개)
4. 사진 업로드 절차 (`images/members/`, 정방형 400px+ 권장)
5. YAML 주의사항: 들여쓰기 스페이스 2칸(탭 금지), 콜론 뒤 스페이스, 특수문자 있으면 따옴표
6. 문제해결: "섹션에 오류 박스가 뜨면" → 표시된 줄 확인, 직전 수정 되돌리기
7. 로컬 미리보기 (선택): `python3 -m http.server 8000`

**DEPLOY.md** (교수님 대상):
1. GitHub 저장소 생성 + push
2. Settings → Pages → main 브랜치 활성화 → `https://<계정>.github.io/<저장소>/`
3. 서버컴 배포: `git clone` 후 nginx/Apache 웹루트 지정 또는 주기적 `git pull` (cron 예시 포함)
4. 커스텀 도메인 연결 (선택)
5. 학생 권한: Collaborator 추가 방법, PR 승인 게이트 전환 방법 (branch protection)

- [ ] **Step 1: 3개 문서 작성**
- [ ] **Step 2: 커밋** — `git commit -m "docs: add update guide and deployment docs"`

---

### Task 7: Playwright 최종 검증

**Files:** 없음 (검증 + 발견 버그 수정)

- [ ] **Step 1: 로컬 서버 기동** — `python3 -m http.server 8000` (background)
- [ ] **Step 2: 데스크톱 1440×900** — 스크린샷: 8개 섹션 전부 렌더, hero 파티클/타이핑/카운트업 동작
- [ ] **Step 3: 모바일 390×844** — 스크린샷: 1열 레이아웃, 햄버거 메뉴, 가로 스크롤 없음
- [ ] **Step 4: 콘솔 검사** — 에러 0 (favicon 404 제외 허용 → 인라인 SVG favicon으로 제거)
- [ ] **Step 5: YAML 오류 주입 테스트** — `news.yml` 깨뜨림 → news 섹션 오류 박스 + 나머지 7개 섹션 정상 → 원복
- [ ] **Step 6: 필터 동작** — Publications 필터 버튼 클릭 → 해당 type만 표시
- [ ] **Step 7: 발견 이슈 수정 후 최종 커밋** — `git commit -m "fix: address issues found in verification"`
