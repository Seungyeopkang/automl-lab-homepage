# AutoML Lab 홈페이지 설계 스펙

날짜: 2026-07-16
상태: 사용자 승인 (Fancy 디자인 강화 요청 반영)

## 목표

경기대학교 AI컴퓨터공학부 서왕덕 교수 AutoML Lab 홈페이지.

- 학부연구생·석사·박사 지원자가 흥미를 느끼는 화려하고(fancy) 현대적인 디자인
- 학생들이 데이터 파일(YAML)만 수정해서 콘텐츠 업데이트 (특히 Members, Publications, Projects, News)
- 로컬 테스트 → GitHub Pages 주력 배포 → 서버컴퓨터 복사 배포 병행 가능
- 국문 위주 (논문 제목·연구실명 등 영문 혼용)

## 기술 결정

**순수 정적 사이트 + 클라이언트 사이드 YAML 렌더링.** 빌드 단계 없음.

- HTML/CSS/JS (프레임워크 없음, js-yaml CDN으로 YAML 파싱)
- 수정 → commit → 즉시 반영. 로컬/GitHub Pages/서버컴 모두 동일 파일
- 로컬 테스트: `python3 -m http.server 8000`
- 거부한 대안: Jekyll (로컬 Ruby 툴체인 부담), Astro+Actions (학생 YAML 오타 → 빌드 실패 리스크)

## 파일 구조

```
homepage/
├── index.html          # 뼈대 — 학생이 건드릴 일 없음
├── css/style.css
├── js/main.js          # YAML 로드 + 렌더링 + 애니메이션
├── data/               # ★ 학생 수정 영역
│   ├── members.yml
│   ├── publications.yml
│   ├── projects.yml
│   └── news.yml
├── images/
│   └── members/        # 멤버 사진
├── UPDATE_GUIDE.md     # 학생용 가이드 (GitHub 웹 편집 절차, 예시, 문제해결)
└── DEPLOY.md           # GitHub Pages 설정 + 서버컴 배포 절차
```

## 페이지 구성 (원페이지 + 스크롤 내비게이션)

1. **Hero** — "AutoML Lab @ 경기대학교" + 한 줄 미션 + 연구 키워드 타이핑 로테이션
2. **Research** — 연구 분야 4 카드 (NAS, 효율적 딥러닝, 특징 선택, 진화 알고리즘) + 학부생 눈높이 설명
3. **Professor** — CV 기반: 사진, 약력, 학력/경력, 수상
4. **Members** — 학생 카드 + "YOU?" 모집 카드
5. **Publications** — 연도별 그룹, 유형/연도 필터, highlight 배지, 랩 멤버 이름 자동 볼드
6. **Projects** — 연구과제 카드 (기간, 지원기관, 상태)
7. **News** — 타임라인
8. **Join Us** — 학부연구생/석사/박사별 안내, 지원 방법, 연락처

콘텐츠 증가 시 Publications 별도 페이지 분리 가능한 구조 유지.

## 데이터 스키마

```yaml
# data/members.yml
- name: 홍길동
  name_en: Gildong Hong
  role: 석사과정          # 교수 | 박사과정 | 석사과정 | 학부연구생 | 졸업생
  photo: images/members/hong.jpg   # 생략 시 이니셜 아바타 자동 생성
  interests: [NAS, 경량화]
  email: hong@kyonggi.ac.kr
  github: gildong          # 선택
  joined: 2025-03
```

```yaml
# data/publications.yml
- title: "..."
  authors: "Wangduk Seo, Jaesung Lee"   # 랩 멤버 자동 볼드
  venue: UAI
  venue_full: "The 40th Conference on Uncertainty in Artificial Intelligence"
  year: 2024
  type: conference        # journal | conference | domestic | patent
  highlight: true         # 대표 논문 배지
  link: https://...       # 선택
```

```yaml
# data/projects.yml
- title: "온디바이스 AI를 위한 신경망 구조 탐색"
  period: 2019.03 – 2022
  funder: 한국연구재단
  description: ...
  status: 진행중           # 진행중 | 완료
```

```yaml
# data/news.yml
- date: 2026-07
  category: paper         # paper | award | member | etc
  text: "Scientific Reports 논문 게재 🎉"
```

초기 데이터: CV_korean.tex에서 추출 (논문 13 국제저널 + 3 국제학회 + 2 국내저널, 특허 3, 과제 5, 수상 2, 교수 약력).

## 학생 업데이트 워크플로우

1. GitHub 저장소 웹 UI에서 `data/` 파일 열기 → 연필 → 수정 → Commit
2. GitHub Pages 1분 내 자동 반영. git 지식 불필요
3. 사진: `images/members/` 웹 업로드 → YAML에 파일명 기재
4. 승인 게이트 원할 시: branch protection + PR 방식으로 전환 가능 (초기엔 직접 commit)

## 오류 안전장치

- YAML 파싱 실패: 해당 섹션에 "데이터 파일 오류 — N번째 항목 근처 확인" 표시, 나머지 섹션 정상 동작
- 필수 필드 누락 항목: 건너뛰고 콘솔 경고
- 사진 404: 이니셜 아바타 폴백

## 디자인 방향 — Fancy (사용자 강화 요청)

관공서/학과 홈페이지 느낌 완전 배제. 시선을 붙잡는 화려함, 단 성능·가독성 유지.

- **다크 테마 베이스**: 딥 네이비/블랙 + 일렉트릭 블루·바이올렛 그라디언트 포인트
- **Hero**: 풀 뷰포트 인터랙티브 신경망 파티클 캔버스 (마우스 반응, 노드 연결선) — AutoML 시각 은유
- **그라디언트 텍스트** 헤드라인 + 연구 키워드 타이핑 로테이션 효과
- **글래스모피즘** 내비게이션 바 + 스크롤 진행 표시
- **통계 카운트업**: 논문 수, 특허, Best Paper 수 — 스크롤 진입 시 애니메이션
- **카드**: 그라디언트 보더 글로우, 호버 리프트/틸트
- **섹션 리빌**: IntersectionObserver 페이드·슬라이드 인
- **News 타임라인**: 애니메이션 도트/라인
- 타이포: Pretendard (국문), 모노스페이스 포인트 (코드 감성)
- 모바일 완전 대응, `prefers-reduced-motion` 존중
- 외부 의존성: js-yaml + Pretendard CDN만. 성능 목표: 첫 로드 1초 내 (파티클은 지연 초기화)

## 배포

- **GitHub Pages 주력**: public 저장소, Settings → Pages. 무료
- **서버컴 병행**: 동일 파일 복사 또는 git pull. DEPLOY.md에 절차 문서화
- 로컬: `python3 -m http.server 8000`

## 테스트

- 로컬 서버 + Playwright: 데스크톱/모바일 뷰포트 렌더링 검증
- YAML 오타 시나리오 → 오류 표시 확인
- 콘솔 에러 0 확인
